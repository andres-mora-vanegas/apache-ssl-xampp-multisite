# apache-ssl-xampp-multisite
This repository explains how to config apache with xampp to have multiple sites in same machine

## httpd-vhost.conf file

Add next config in the final line of the file:

```
<VirtualHost *:80>
     DocumentRoot "C:/xampp/htdocs/opensky-information/proyectos/bisonte/wp"
     ServerName example.site.com
 </VirtualHost>
 <VirtualHost *:443>
     DocumentRoot "C:/xampp/htdocs/opensky-information/proyectos/bisonte/wp"
     ServerName example.site.com
     SSLEngine on
     SSLCertificateFile "crt/example.site.com/server.crt"
     SSLCertificateKeyFile "crt/example.site.com/server.key"
 </VirtualHost>
 <VirtualHost *:80>
     DocumentRoot "C:/xampp/htdocs/cela_ui"
     ServerName subdomain.example.site.com 
 </VirtualHost>
 <VirtualHost *:443>
     DocumentRoot "C:/xampp/htdocs/cela_ui"
     ServerName subdomain.example.site.com
     SSLEngine on
     SSLCertificateFile "crt/subdomain.example.site.com/server.crt"
     SSLCertificateKeyFile "crt/subdomain.example.site.com/server.key"
 </VirtualHost>
```

## Add following content of windows host file located in C:\Windows\System32\drivers\etc\etc

```
127.0.0.1 latam.medtronicacademy.com
127.0.0.1 cela.latam.medtronicacademy.com
```

If need to generate the ssl files you can use the next tutorial:

https://shellcreeper.com/how-to-create-valid-ssl-in-localhost-for-xampp/

## Cert.conf file

```
[ req ]

default_bits        = 2048
default_keyfile     = server-key.pem
distinguished_name  = subject
req_extensions      = req_ext
x509_extensions     = x509_ext
string_mask         = utf8only

[ subject ]

countryName                 = Country Name (2 letter code)
countryName_default         = US

stateOrProvinceName         = State or Province Name (full name)
stateOrProvinceName_default = NY

localityName                = Locality Name (eg, city)
localityName_default        = New York

organizationName            = Organization Name (eg, company)
organizationName_default    = Example, LLC

commonName                  = Common Name (e.g. server FQDN or YOUR name)
commonName_default          = site.test

emailAddress                = Email Address
emailAddress_default        = test@example.com

[ x509_ext ]

subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid,issuer

basicConstraints       = CA:FALSE
keyUsage               = digitalSignature, keyEncipherment
subjectAltName         = @alternate_names
nsComment              = "OpenSSL Generated Certificate"

[ req_ext ]

subjectKeyIdentifier = hash

basicConstraints     = CA:FALSE
keyUsage             = digitalSignature, keyEncipherment
subjectAltName       = @alternate_names
nsComment            = "OpenSSL Generated Certificate"

[ alternate_names ]

DNS.1       = site.test
```

## make-cert.bat file
```
@echo off
set /p domain="Enter Domain: "
set OPENSSL_CONF=../conf/openssl.cnf

if not exist .\%domain% mkdir .\%domain%

..\bin\openssl req -config cert.conf -new -sha256 -newkey rsa:2048 -nodes -keyout %domain%\server.key -x509 -days 3650 -out %domain%\server.crt

echo.
echo -----
echo The certificate was provided.
echo.
pause
```

Thanks to https://shellcreeper.com/ for the tutorial
![tutorial](tutorial-in-image.png?raw=true "Tutorial")
