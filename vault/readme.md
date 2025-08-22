helm repo add hashicorp https://helm.releases.hashicorp.com
helm search repo hashicorp/vault
helm install vault hashicorp/vault --values override-values.yml -n vault
kubectl port-forward vault-0 8200:8200

#Log into vault terminal
vault operator unseal