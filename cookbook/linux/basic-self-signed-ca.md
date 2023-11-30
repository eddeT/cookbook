# Basic self signed CA

```bash
openssl genrsa -aes256 -out <ca-name>.key 4096
openssl req -x509 -new -nodes -key <ca-name>.key -sha256 -days 3650 -out <ca-name>.pem

openssl genrsa -aes256 -out wildcard-<domain-name>.key 2048
openssl req -new -key wildcard-<domain-name>.key -extensions v3_ca -out wildcard-<domain-name>.csr

vi openssl.ss.conf
basicConstraints=CA:FALSE
subjectAltName=DNS:*.<domain.name>
extendedKeyUsage=serverAuth

openssl x509 -req -in wildcard-<domain-name>.csr -CA <ca-name>.pem -CAkey <ca-name>.key -CAcreateserial -extfile openssl.ss.conf -out wildcard-<domain-name>.crt -days 365 -sha256
```