■オレオレ証明書
```
openssl genrsa 2048 > server.key
openssl req -new -key server.key > server.csr
openssl x509 -days 3650 -req -signkey server.key < server.csr > server.crt
openssl x509 -in server.crt -out server.der -outform DER
openssl x509 -in server.der -inform DER -out server.pem -outform pem
```
