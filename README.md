# rbac-tutorial


## Adding Users

The genrsa command generates an RSA private key.

```
openssl genrsa -out hanifa-test.pem 2048

openssl req -new -key hanifa-test.pem -out hanifa-test.csr -subj "/CN=hanifa-test"

```
