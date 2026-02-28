## Configuration for CD3-02 – Create Self-Signed TLS Certificate (PoC)

### 1. Generate Certificate (Git Bash / WSL)

If using Git Bash on Windows, disable automatic path conversion:

```bash
export MSYS_NO_PATHCONV=1
```

#### Generate the self-signed certificate:
```bash
mkdir -p certs && cd certs

openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout bankofanthos.local.key \
  -out bankofanthos.local.crt \
  -subj "/CN=bankofanthos.local" \
  -addext "subjectAltName=DNS:bankofanthos.local"
```

This creates:

```code
certs/
├── bankofanthos.local.crt
└── bankofanthos.local.key
```
### 2. Create Kubernetes TLS Secret
Return to the project root:
```bash
kubectl delete secret boa-tls --ignore-not-found
```
Delete any existing secret
```bash
kubectl delete secret boa-tls --ignore-not-found
```
Create the TLS secret in Kubernetes:
```bash
kubectl create secret tls boa-tls \
  --cert=certs/bankofanthos.local.crt \
  --key=certs/bankofanthos.local.key
```
verify
```bash
kubectl get secret boa-tls
```
