# Compiling a Slimmer OpenSSL from Source (Ubuntu)
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
make -j$(nproc)
make -j$(nproc) install_sw
ldconfig
```
* `make -j$(nproc)` will parallelize compilation (in English: "compile using the number of threads this system has)
* `make -j$(nproc) install_sw` is also parallel, and installs without docs (omit the `_sw` to include docs)

By default, your OpenSSL binary will be located in /usr/local/ssl. It may be necessary to run `ldconfig` to get it up and running.
