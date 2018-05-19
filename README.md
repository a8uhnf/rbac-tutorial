# rbac-tutorial


## Adding Users

The genrsa command generates an RSA private key.

```
openssl genrsa -out hanifa-test.pem 2048

openssl req -new -key hanifa-test.pem -out hanifa-test.csr -subj "/CN=hanifa-test"

```


### Certificate Signing Request with K8s

```yaml

apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: user-request-jakub
spec:
  groups:
  - system:authenticated
  request: <CSR>
  usages:
  - digital signature
  - key encipherment
  - client auth
```

