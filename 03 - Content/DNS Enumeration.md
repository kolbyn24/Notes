---
creation date: November 20th 2024
last modified date: November 20th 2024
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[DNS Enumeration]]  
___

## Description:  

The goal of DNS enumeration is to take a top level domain and find either sub-domains or records containing CNAMES that point to either interesting servers or domains that can be taken over (DNS name that can be purchased or sub-domain takeover with some sort of cloud environment)

### Tools:

cert.sh - website that checks DNS records
wayback urls - local tool that uses the wayback machine to find old records based on a domain name
brute_subs.sh - Patrick Higgins wrote this script to automate subdomain brute forcing
owasp amass - github page that uses other techniques to find subdomains
zdns - for bulk resolving DNS names.

------

### Process:
Set the top level domain (bash):
```
export DOMAIN=example.com
```
Use crt.sh
```
#This command will give you the raw HTML respnse
curl "https://crt.sh/?q=$DOMAIN" -o "${DOMAIN}_crt_sh.txt"
#This command will sort the HTML and give you a clean list of sub domains
grep '<TD>' "${DOMAIN}_crt_sh.txt" | grep -Po "[^>]+$DOMAIN" | sort -u > "${DOMAIN}_crt_sh_sorted.txt"
```
You can attempt to resolve these either by opening them in firefox or using eyewitness.
```
# Check the number of lines, be careful opening large files in firefox
wc -l "${DOMAIN}_crt_sh_sorted.txt"
# Open in Firefox
cat "${DOMAIN}_crt_sh_sorted.txt" | xargs firefox
# Take screenshots with eyewitness
./EyeWitness -f "${DOMAIN}_crt_sh_sorted.txt" --web
```

Install wayback URLs
```
$ go install github.com/tomnomnom/waybackurls@latest
$ sudo ln -s ~/go/bin/waybackurls /usr/local/bin/
```
Use it with the following:
```
export DOMAIN=example.com
echo "$DOMAIN" | waybackurls | tee "$DOMAIN_waybackurls.txt"

# You can bulk run it with the following:
cat domains.txt | while read dn; do echo "##### $dn #####"; echo "$dn" | waybackurls | tee "${dn}_waybackurls.txt"; echo; done

# Once you have the output you can sort it with
ls *_waybackurls.txt | while read f; do export dn=$(echo "$f" | sed 's/_waybackurls.txt//'); echo "##### $dn #####"; grep -Po "^.+?$dn" "$f" | sed -r 's/^http.?:\/\///' | sort -u | tee -a "${dn}_potential_subs.txt"; echo; done
```

To run amass and do a brute force to enumerate subdomains you can use this script that Patrick Higgins wrote:
usage: `amass_and_brute_subs.sh domain subdomain_file.txt`
see best-dns-wordlist.txt from https://wordlists.assetnote.io/ for a good word list.

```
#!/bin/bash
#
# amass_and_brute_subs.sh
#
# Create lists of potential subdomains from a list of parent domains.
# Uses OWASP amass and a given wordlist
#
# Note: For a good subdomain wordlist, see best-dns-wordlist.txt from https://wordlists.assetnote.io/
#

domains_file="$1"
wordlist="$2"
if [[ ! -r "$domains_file" || ! -r "$wordlist" ]]; then
    echo 'Usage:' >&2
    echo "    $0 <domains_file> <wordlist>" >&2
    exit 1
fi

# check for amass in PATH
if [[ -z $(which amass) ]]; then
    echo 'amass not found in PATH' >&2
    exit 2
fi

# begin main loop
for domain in $(cat "$domains_file"); do
    echo "$domain ..."

    # try not to clobber output file
    output_file="$domain"_potential_subs.txt
    if [[ -f "$output_file" ]]; then
        printf "Output file $output_file already exists. Overwrite? (Y/n) "
        read choice
        if [[ "$choice" == 'n' || "$choice" == 'N' ]]; then
            echo 'Exiting'
            exit 3
        fi
    fi

    # run amass, append output to file
    amass enum -silent -passive -timeout 2 -d "$domain" -o "$output_file"

    # cycle through wordlist, append potential subs to file
    for sub in $(cat "$wordlist"); do
        echo "${sub}.${domain}" >> "$output_file"
    done

    # add root domain to file as well for further resolution tools
    echo "$domain" >> "$output_file"

    # remove duplicates
    sort --unique --output="$output_file" "$output_file"

done

echo 'Completed. Output files stored in format: DOMAIN_potential_subs.txt'


```
The output of this script is messed up every since amass changed their output so you can use this to clean it up:
```
cat rhspgov.com_potential_subs.txt | grep -v ' '
```

Once you have a complete DNS list, you can use the following script to bulk resolve the DNS names (uses zDNS):
bulk_resolve_dns.sh
```
#!/bin/bash
#
# bulk_resolve_dns.sh
#
# Takes a list of DNS names and runs them through zdns [with
# the specified query type] to attempt to resolve them. Creates
# a list of resolvable ones (any answer counts, including a 
# dangling CNAME)
#

# args
dns_names_file="$1"
query_type="$2"
if [[ ! -r "$dns_names_file" ]]; then
    echo 'Usage:' >&2
    echo "    $0 <dns_names_file>.txt [query_type]" >&2
    exit 1
fi
if [[ -z "$query_type" ]]; then
    query_type='A'
fi

# confirm zdns in PATH
if [[ -z $(which zdns) ]]; then
    echo 'zdns not found in PATH' >&2
    exit 2
fi

# create output filenames
base_output_file="$(basename $dns_names_file | sed 's/\.txt//')"_$query_type
zdns_output_file="$base_output_file.json"

# build zdns flags
zdns_flags=("$query_type")
zdns_flags+=('--name-servers' '1.1.1.1,1.0.0.1,8.8.8.8,8.8.4.4,9.9.9.10,149.112.112.10')
zdns_flags+=('--result-verbosity' 'short')
zdns_flags+=('--verbosity' '2')
zdns_flags+=('--threads' '100') # default 1,000
zdns_flags+=('--retries' '2')   # default 1
zdns_flags+=('--output-file' "$zdns_output_file")

# resolve DNS names
cat "$dns_names_file" | zdns "${zdns_flags[@]}"
echo "zdns output saved to:           $zdns_output_file"

# extract resolvable names from zdns file
resolvable_file="${base_output_file}_resolvable.txt"
jq --raw-output 'select(.data.answers) | .name' "$zdns_output_file" | sort -u > "$resolvable_file"
echo "Resolvable DNS names saved to:  $resolvable_file"
```
If you give it the zdns JSON file that comes out of bulk_resolve_dns.sh, The following script will make a helpful little visual showing you how CNAMEs resolve:
```
#!/bin/bash
#
# cname_chains.sh
#
# Parse a zdns JSON file and show CNAME resolution chains.
#

json_file="$1"
if [[ ! -f "$json_file" ]]; then
    echo 'Usage:' >&2
    echo "    $0 <zdns_output.json>" >&2
    exit 1
fi

grep -i cname "$json_file" | while read line; do
    cname_record=$(echo "$line" | jq -r .name)

    # Print the original CNAME DNS record
    echo -n "$cname_record"

    # Print additional CNAME records
    echo "$line" | jq -r '.data.answers[] | select(.type=="CNAME") | .answer' | while read answer; do
        echo -n " -> $answer"
    done

    # Print A and AAAA records
    ip_records=$(echo -n "$line" | jq -r '.data.answers[] | select([.type] | inside(["A", "AAAA"])) | .answer' | tr '\n' ',' | sed 's/,$//')
    echo " -> $ip_records"

done | sort -u | column -t


```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 20th 2024 (03:37 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
