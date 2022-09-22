---
creation date: January 3rd 2022
last modified date: January 3rd 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: [[Phishing]]
Search Tag: #📖  

# [[Phishing Payloads]]  

To get csharp shellcode from .bin files, use [[HexDump]]

- [ ] Built-in CobaltStrike Payloads
	- [ ] HTA
~~- [ ] CactusTorch~~
- [ ] HTA
	- [ ] MSBuild
	- [ ] MSBuild WebDAV - python 
- [ ] [[Scarecrow]] - HTA
- [ ] [[ExcelNTDonut]]
- [ ] Macrome
- [ ] [[Regasm]]
- [ ] [[Ivy]]
- [ ] [[ScareCrow]]
- [ ] ISO
	- [ ] LNK -> [[Regasm]] 
- [ ] Password Protected Word Doc
- [ ] BananaPhone 
- [ ] VSTO Office Files
- [ ] DueDLLigence
- [ ] [[Mangle]]
- [ ] FrostByte

### Payload Testing Doc Format
```
#,Filename,C2 Protocol (HTTPS/DNS),Technique,Shellcode Architecture (x64/x86),Staged/Stageless,Shellcode Injection (Local / Remote),File Hash (MD5),File Hash (SHA256), Relevant Notes
01,01-blah.exe,HTTPS,
```

### Generate Hashes for Payloads
```bash
#!/bin/bash

echo "#,Filename,C2 Protocol (HTTPS/DNS),Technique,Shellcode Architecture (x64/x86),Staged/Stageless,Shellcode Injection (Local / Remote),File Hash (MD5),File Hash (SHA256), Relevant Notes"

while read -r line; do
	PAYNUM=$(echo -n $line | cut -f 1 -d '-')
	PROTOCOL=$(echo -n $line | cut -f 4 -d '-')
	ARCH=$(echo -n $line | cut -f 5 -d '-' | cut -f 1 -d '.')
	TECHNIQUE=$(echo -n $line | cut -f 2-3 -d '-')
	MD5=$(md5sum $line | cut -f 1 -d ' ')
	SHA256=$(sha256sum $line | cut -f 1 -d ' ')
	
	if echo -n $line | grep -iE '(remote|inject)' > /dev/null 2>&1; then 
		REMOTE="Remote"
	else
		REMOTE="Local"
	fi
	RESULT="$PAYNUM,$line,$PROTOCOL,$TECHNIQUE,$ARCH,Stageless,$REMOTE,$MD5,$SHA256,"
	echo $RESULT;
done


```

## Payload Testing Page 
Use the following aggressor script (gator-host.cna) to host all payloads along with a testing splash page 

```bash
# Author: Adam Brown
# Usage:
#    from cobaltstrike directory run
#      ./agscript teamserver 50050 user pass path/to/gator-host.cna

$URL_PREFIX = "/cisatest/";
$PAYLOAD_DIRECTORY = "/share/Working/payloads/";
$SUPPORTED_EXTENSIONS = "exe|hta|iso|xls|doc|js|zip|cpl";
$FORCE_DOWNLOAD_EXTENSIONS = "xls|js|hta";
$DEBUG_LEVEL = 1; # 2 for debug, 8 for verbose, 128 for everything possible

sub hostFiles {
	#list files to host
	local('$fh @path $filepath $mimetype $filename $extension $content');

	$payload_hosting_file_name = $PAYLOAD_DIRECTORY . "index.html";
	printInfo("Writing payload test hosting page to $payload_hosting_file_name ...\n");
	$phf = openf(">" . $payload_hosting_file_name);

	printTestPageHeader($phf);
	
	printDebug("Reading file list...");
	@files = `find $PAYLOAD_DIRECTORY -maxdepth 1 -type f`;
	@files = sorta(@files);
	
	# parse listener names from output
	# use a pseudo-index because sleep cant take *s in its executed commands to gather files
	$psuedoIndex = 1;
	
	foreach $index => $filepath (@files){
		if (!-exists $filepath) {
			printInfo("$filepath does not exist...\n");
			continue;
		}

		@path = split('/', $filepath);
		$filename = @path[-1];
		@fsplit = split('\.', $filename);
		$extension = @fsplit[-1];

		$mimetype = "automatic";
		$content = "";

		if ($extension !isin $SUPPORTED_EXTENSIONS) { 
			printError("Skipping $filename due to unsupported extension...\n");
			continue;
		} else if ($extension isin $FORCE_DOWNLOAD_EXTENSIONS) {
			$mimetype = "application/octet-stream";
		}

		printDebug("Hosting $filepath ...");

		$fh = openf($filepath);
		if (checkError($error)) {
		   printError("Could not open file: $error \n");
		} else {
			reset($fh);
			$content = readb($fh, -1);
			closef($fh);
		}

		printDebug("Read " . strlen($content) . " / " . lof($filepath) . " bytes from $filepath");

		site_host(localip(), 443, $URL_PREFIX . "$filename", $content, $mimetype, "Serves $filepath", true);
		printInfo("Hosted: $filename (" . strlen($content) . " bytes) as $mimetype \n");
		println($phf, '<div class="col-sm-3">');
		println($phf, '<a class="btn btn-primary btn-lg" href="' . $URL_PREFIX . $filename . '" role="button" target="_blank">' . $psuedoIndex . '</a>');
		println($phf, '</div>');

		if (($psuedoIndex % 3) == 0) {
			println($phf, '<div class="col-sm-1"></div>');
			println($phf, '</div>');
        	println($phf, '<div class="row" style="text-align: center;min-height: 85px">');
            println($phf, '<div class="col-sm-1"></div>');
		}
		$psuedoIndex += 1;
	}

	$remainder = 11 - (($psuedoIndex % 3) * 3);

	println($phf, '<div class="col-sm-' . $remainder . '"></div>');

	println($phf, '</div>'); 

	printTestPageFooter($phf);
	closef($phf);

	$fh = openf($payload_hosting_file_name);
	$content = readb($fh, -1);
	closef($fh);

	site_host(localip(), 443, $URL_PREFIX, $content, "automatic", "Phishing Payload Test Page", true);
}

sub printInfo {
	println("[+] $1");
}

sub printDebug {
	if (debug() > 1) {
		println("[-] $1");
	}
}

sub printError {
	println("[!] $1");
}

sub printTestPageHeader {
	println($1, '<!DOCTYPE html>');
	println($1, '<html>');
	println($1, '<head>');
	println($1, '<meta charset="utf-8">');
	println($1, '<meta http-equiv="X-UA-Compatible" content="IE=edge">');
	println($1, '<meta name="viewport" content="width=device-width, initial-scale=1">');
	println($1, '<title>Payload Testing</title>');
	println($1, '<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">');
	println($1, '<script src="https://kit.fontawesome.com/9bc9872b8b.js"></script>');
	println($1, '<style type="text/css">');
	println($1, 'slow-spin {animation-duration: 6s;}');
	println($1, '</style>');
	println($1, '</head>');
	println($1, '<body style="background-color: #3f3f3f">');
	println($1, '<div class="container-fluid">');
	println($1, '<div class="jumbotron" style="max-height: 275px">');
	println($1, '<h1>Phishing Payload Testing!</h1>');
	println($1, '<p class="lead">This site is used for the simple task of validating which payloads will execute in the environment with security in place.</p>');
	println($1, '<p class="lead">Please use the following browser:  <i class="fab fa-internet-explorer fa-2x" style="color: #1EBBEE"></i>    <span class="fa-stack fa-2x"><i class="fab fa-chrome fa-stack-1x"></i><i class="fas fa-ban fa-stack-2x slow-spin" style="color: Red;opacity: 0.5"></i></span></p>');
	println($1, '</div>');
	println($1, '<div class="row" style="text-align: center;min-height: 85px">');
	println($1, '<div class="col-sm-1"></div>');
}

sub printTestPageFooter {
	println($phf, '</div>');
	println($phf, '</body>');
	println($phf, '</html>');
}

on ready {
	debug($DEBUG_LEVEL);
	hostFiles();
	println("Done. Press Ctrl-C to exit"); 
	# closing client happens pre-maturely, just ctrl+c it 
}

```

## Spear Phishing Weakness Finding

### Import Phishing Payload Data to PenTest Portal

Run the following python script pointing it to the directory holding your phishing payloads. It will output CSV data which can be uploaded directly to the PTP via the `/admin`  interface
```python
#!/env/usr/python

import sys
import pathlib
from datetime import datetime

# #echo "#,Filename,C2 Protocol (HTTPS/DNS),Technique,Shellcode Architecture (x64/x86),Staged/Stageless,Shellcode Injection (Local / Remote),File Hash (MD5),File Hash (SHA256), Relevant Notes"
# echo "id,created_at,updated_at,payload_description,c2_protocol,border_protection,host_protection,order"
# #1,2022-09-20 13:21:51,2022-09-20 13:21:51,Linked Macro Embedded Microsoft Word Document,HTTPS,N,B,1
# #31-scarecrow-control_sandbox_noamsi-https-x64.js 

class Payload:

	def __init__(self):
		self.id = 0
		self.create_time = None
		self.update_time = None
		self.description = ''
		self.protocol = 'HTTPS'
		self.border_protection = 'N'
		self.host_protection = 'N'
		self.order = '0'
	
	def set_description(self, framework, description, architecture, extension):
		self.description = f"{framework.title()} [{extension.upper()}] - "
		self.description += f"{description.replace('_', ' ').title()} "
		self.description += f"({architecture.lower()})"

	def __str__(self):
		result = f"{self.id},{self.create_time},{self.update_time},"
		result += f"{self.description},{self.protocol},{self.border_protection},"
		result += f"{self.host_protection},{self.order}"

		return result

def write_csv(payloads):
	print(
		"id,created_at,updated_at,payload_description,c2_protocol,"
		"border_protection,host_protection,order"
	)
	for payload in payloads:
		print(payload)

def read_payloads(payload_dir):
	payloads = []
	timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

	for file in pathlib.Path(payload_dir).iterdir():
		payload = Payload()

		if file.name == "index.html":
			continue
		filename = file.stem.split('-')
		extension = file.name.split('.')[-1]

		payload.id = len(payloads)
		payload.create_time = timestamp
		payload.update_time = timestamp
		payload.set_description(filename[1], filename[2].replace('_', ' '), filename[4], extension)
		payload.protocol = filename[3].upper()
		payload.border_protection = 'N'
		payload.host_protection = 'N'
		payload.order = len(payloads)

		payloads.append(payload)
	
	return payloads


if __name__ == "__main__":
	payloads = read_payloads(sys.argv[1])
	write_csv(payloads)
```

To be able to export data from PentestPortal
```bash 
# If it's not running yet, it might work to do this and then start as normal
echo "tablib==0.14.0" >> ptp/docker/prod/requirements.txt 

# If it's already running, the below steps should work
docker exec -it prod-web-1 'pip uninstall tablib'
docker exec -it prod-web-1 'pip install tablib==0.14.0'

docker compose -f docker-compose.prod.yml -p prod build web
docker compose -f docker-compose.prod.yml -p prod restart web

# old - 
# pip install xlrd xlwt odfpy pyyaml markuppy 
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 3rd 2022 (10:49 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
