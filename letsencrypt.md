# Configuring Strong SSL/TLS with Apache and Let's Encrypt
### Generate DHparam First
```
cd /etc/ssl/certs
openssl dhparam -out dhparam.pem 4096
```

### Install the Dependencies
```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot python3-certbot-apache
```

### Obtain the Certificate
```
certbot --apache --domain <YOUR_DOMAIN> --agree-tos --redirect --hsts --uir --staple-ocsp --rsa-key-size 4096
```

### Tweak Your SSL Configuration
```
vi vi /etc/letsencrypt/options-ssl-apache.conf
SSLProtocol +all -SSLv2 -SSLv3 -TLSv1 -TLSv1.1
SSLOpenSSLConfCmd DHParameters "/etc/ssl/certs/dhparam.pem"
```

### Reload Apache
```
systemctl reload apache2.service
```
