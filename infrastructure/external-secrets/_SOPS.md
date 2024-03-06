# Encrypting files with Mozilla SOPS and age

Study the README.md file at the root of this repository for information about how to get the encryption software.

Also check the <https://external-secrets.io/latest/provider/oracle-vault/> for the relevant vault provider. I use Oracle Vault.

## secret-sops-encrypted.yaml

Create a file called secret.yaml with content like this:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: oracle-secret
  namespace: external-secrets
  labels:
    type: oracle
type: Opaque
stringData:
  privateKey: |
   REDACTED
  fingerprint: REDACTED
```

Encrypt it with sops and age:

```bash
AGE_PUBLIC_KEY=$(grep "public key" age.key | cut -d ":" -f2 | tr -d " ")

sops --age=$AGE_PUBLIC_KEY --encrypt --encrypted-regex '^(data|stringData)$' secret.yaml > secret-sops-encrypted.yaml
```

## cluster-secret-store-sops-encrypted.yaml

Create a file called cluster-secret-store.yaml with content like this:

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ClusterSecretStore
metadata:
  name: oracle-vault
  namespace: external-secrets
spec:
  provider:
    oracle:
      vault: REDACTED
      region: eu-frankfurt-1
      auth:
        user: REDACTED
        tenancy: REDACTED
        secretRef:
          privatekey:
            name: oracle-secret
            namespace: external-secrets
            key: privateKey
          fingerprint:
            name: oracle-secret
            namespace: external-secrets
            key: fingerprint
```

Encrypt it with sops and age:

```bash
AGE_PUBLIC_KEY=$(grep "public key" age.key | cut -d ":" -f2 | tr -d " ")

sops --age=$AGE_PUBLIC_KEY --encrypt --encrypted-regex '^(spec)$' cluster-secret-store.yaml > cluster-secret-store-sops-encrypted.yaml
```
