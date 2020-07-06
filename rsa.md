# RSA

## Create CA Certificate
Here, we aren't encrypting the private key (`-nodes` means "no DES"). This isn't recommended in production.
```
openssl genrsa -out ca.key 2048
```
Generate a self-signed certificate. 
* `-x509` outputs self-signed certificate instead of a certificate signing request and 
* `-nodes` says not to encrypt to private key, if applicable.
```
openssl req -x509 -new -sha256 -nodes -key ca.key  -days 3650 -out ca.crt -subj "/C=US/ST=AZ/L=Tempe/O=SW/CN=ca.demo"
```
## Create and Sign Host Certificate
Generate a private key for a host.
```
openssl genrsa -out host.key 2048
```
Use the host's private key to generate a CSR. We omit `-x509` because we want a CSR, not a self-signed certificate.
```
openssl req -new -key host.key -out host.csr -nodes -subj "/C=US/ST=AZ/L=Tempe/O=SW/CN=host.demo"
```
Use the CA and client CSR to generate a signed key.
```
openssl x509 -req -sha256 -in host.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out host.crt -days 730  
```