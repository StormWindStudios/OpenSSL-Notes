# ECDSA 

## Create CA Certificate
List and select a curve (prime256v1 is a good option).
```
openssl ecparam -list_curves | less
```
Generate a private key for the CA.
```
openssl ecparam -genkey -name prime256v1 -out ca.key
```
Generate a self-signed certificate. 
* `-x509` outputs self-signed certificate instead of a certificate signing request 
* `-nodes` says not to encrypt to private key, if applicable.
```
openssl req -x509 -new -sha256 -nodes -key ca.key -days 3650 -out ca.crt -subj "/C=US/ST=AZ/L=Tempe/O=SW/CN=ca.demo"
```

## Create and Sign Host Certificate
Generate a private key for a host.
```
openssl ecparam -genkey -name prime256v1 -out host.key
```
Use the private key to create a certificate signing request (CSR). We omit `-x509` because we want a CSR, not a self-signed certificate.
```
openssl req -new -sha256 -key host.key -nodes -out host.csr -subj "/C=US/ST=AZ/L=Tempe/O=SW/CN=host.demo"
```
Use the CA to create a signed certificate for this CSR.
```
openssl x509 -req -sha256 -days 730 -in host.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out host.crt
```
(Optionally) convert the host's private key to PKCS#8, which some servers may require.
```
 openssl pkcs8 -topk8 -in host.key -out host-pkcs8.key -nocrypt 
```
