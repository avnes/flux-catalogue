# flux-catalogue

A catalogue over the Helm charts and Git repositories I use in my Flux CD setup on Kubernetes

## Decryption

Create a secret in the flux-system namespace with your AGE decryption private key.

```bash
cat age.agekey |
kubectl create secret generic sops-age \
--namespace=flux-system \
--from-file=age.agekey=/dev/stdin
```
