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
  name: user-request-hanifa-test
spec:
  groups:
  - system:authenticated
  request: <CSR>
  usages:
  - digital signature
  - key encipherment
  - client auth
```

Use below command to encrypt csr into one line.
```
cat hanifa-test.csr | base64 | tr -d '\n'
```

```
kubectl certificate approve user-request-hanifa-test
```

```
kubectl get csr user-request-hanifa-test -o jsonpath='{.status.certificate}' | base64 --decode > hanifa-test.crt
```

### Entry to Config file


```
kubectl config set-cluster hanifa-test --insecure-skip-tls-verify=true --server=<server-ip>


kubectl config set-credentials hanifa-test --client-certificate=hanifa-test.crt --client-key=hanifa-test.pem --embed-certs=true

kubectl config set-context hanifa-test --cluster=hanifa-test --user=hanifa-test

kubectl config use-context hanifa-test

```

Create ClusterRoleBinding with default `view` ClusterRole

```yaml

kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: hanifa-test-view-all
subjects:
- kind: User
  name: hanifa-test
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: view
  apiGroup: rbac.authorization.k8s.io
```
