# Automatic PKCS12 Conversion for Lets Encrypt Certificates
## Setup
0) Install Certbot
1) Specify the path and password for the .pfx file.
2) Place this script in /etc/letsencrypt/renewal-hooks/deploy/auto_pfx.sh:

```
#!/bin/bash
# Adjust these variables as necessary

# Where you want to final PKCS12 file to be stored.
CERT_PATH="/opt/app/certificate.pfx"

# Password to encrypt the PKCS12 file.
CERT_PW="ShoobyDooby"

# Path to LE files, RENEWED_LINEAGE provided by CertBot
PRIV_KEY_PEM="$RENEWED_LINEAGE/privkey.pem"
CERT_PEM="$RENEWED_LINEAGE/cert.pem"
CHAIN_PEM="$RENEWED_LINEAGE/chain.pem"

# If there's already a .pfx file, back it up
if [[ -f "$CERT_PATH" ]]; then
    now=`date +%Y-%m-%d-%T`
    mv $CERT_PATH $CERT_PATH.bak.$now
fi

# Le Conversion
openssl pkcs12 -export -out $CERT_PATH -inkey $PRIV_KEY_PEM -in $CERT_PEM -certfile $CHAIN_PEM -password pass:$CERT_PW
```
3) Don't forget to `chmod`! If the script isn't executable, it's ignored.
4) Request a certificate: `certbot certonly --standalone`
5) Use the OpenSSL command below to create the initial certificate, or be lazy: `certbot renew --force-renew`. On renewal, the output should include:
```
--- snip ---
Renewing an existing certificate
Running deploy-hook command: /etc/letsencrypt/renewal-hooks/deploy/auto_pfx.sh
--- snip ---
```
6) Make sure it worked:
```
shane@serverboi~$ ls -al /opt/app/
drwxr-xr-x 2 root root 4096 Aug  6 18:40 .
drwxr-xr-x 3 root root 4096 Aug  6 18:23 ..
-rw------- 1 root root 5629 Aug  6 18:40 certificate.pfx
```
7) Future renewals should cause the existing .pfx files to be backed up:
```
shane@serverboi~$ ls -al /opt/app/
drwxr-xr-x 2 root root 4096 Aug  6 18:40 .
drwxr-xr-x 3 root root 4096 Aug  6 18:23 ..
-rw------- 1 root root 5629 Aug  6 18:40 certificate.pfx
-rw------- 1 root root 5629 Aug  6 18:31 certificate.pfx.bak...
```
