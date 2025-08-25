kubectl apply -f configmap.yaml -n argocd
kubectl apply -f secret.yaml -n argocd
kubectl patch deployment argocd-repo-server -n argocd --patch-file patch-deployment.yaml

# Vault
vault policy write secret-policy - <<EOF
# Allow reading secrets for your applications
path "secret/data/*" {
  capabilities = ["read", "list"]
}
path "secret/metadata/*" {
  capabilities = ["read", "list"]
}
# Add other paths as needed
EOF

vault token create -policy=secret-policy