---
creation date: February 2nd 2022
last modified date: February 2nd 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: [[SSL]]
Search Tag: #📖  

# [[SSL Certificates]]  
## Using Included Certificates 
The certificate store is located on each teamserver in `/tools/Malleable-C2-Profiles/normal/domain.store`  and the password is in `/tools/Malleable-C2-Profiles/normal/amazon.profile`

To extract from .store to a form useable with Apache and GoPhish:

```bash
#!/bin/bash 

echo -n "What is the domain name (domain.com)? "
read DOMAIN
srcpass=$(tail -n 3 /tools/Malleable-C2-Profiles/normal/amazon.profile | grep 'password' | cut -f 2 -d '"')

keytool -importkeystore -srckeystore /tools/Malleable-C2-Profiles/normal/$DOMAIN.store -destkeystore $DOMAIN.pfx -deststoretype PKCS12 -srcstorepass $srcpass -deststorepass $srcpass

openssl pkcs12 -in $DOMAIN.pfx -passin pass:$srcpass -nocerts -nodes | sed -ne '/-BEGIN PRIVATE KEY-/,/-END PRIVATE KEY-/p' > $DOMAIN.key
openssl pkcs12 -in $DOMAIN.pfx  -passin pass:$srcpass -clcerts -nokeys | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > $DOMAIN.cer
openssl pkcs12 -in $DOMAIN.pfx  -passin pass:$srcpass -cacerts -nokeys -chain | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > $DOMAIN-ca.cer

cat $DOMAIN.cer $DOMAIN-ca.cer > $DOMAIN-fullchain.pem
```





___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: February 2nd 2022 (07:35 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
