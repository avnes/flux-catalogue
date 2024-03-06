# flux-catalogue

A catalogue over the Helm charts and Git repositories I use in my Flux CD setup on Kubernetes

## Encryption

Some of the file in this repo is encrypted using Mozilla SOPS CLI and age CLI.

### Getting the encryption software

1. Read <https://fluxcd.io/flux/guides/mozilla-sops/> to learn how to get SOPS CLI installed
2. Get age CLI from <https://github.com/FiloSottile/age>

### Create an age key pair

```bash
age-keygen -o age.key
```

Save your age key pair somewhere safe. For example in a password manager.

## Decryption

Create a secret in the flux-system namespace with your AGE decryption private key.

```bash
cat age.agekey |
kubectl create secret generic sops-age \
--namespace=flux-system \
--from-file=age.agekey=/dev/stdin
```
