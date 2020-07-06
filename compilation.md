# Compiling OpenSSL from Source (Ubuntu)
## Install dependencies
`sudo apt-get install build-essential`
## Download and extract the sources
```
wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz
tar -zxf openssl-1.1.1g.tar.gz
```
## Configure OpenSSL
```
cd openssl-1.1.1g
./config no-weak-ssl-ciphers no-ssl2 no-ssl3 no-idea no-psk no-srp
```
* `no-weaks-ssl-ciphers` disables RC4
* `no-ssl2` disables SSLv2
* `no-ssl3` disables SSLv3
* `no-idea` disables IDEA algorithm
* `no-psk` disables pre-shared key authentication
* `no-srp` disabled secure remote password authentication

## Compile and Install
```
make
make install
```
By default, your OpenSSL binary will be located in /usr/local/ssl 
