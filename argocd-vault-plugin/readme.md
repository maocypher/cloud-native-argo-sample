kubectl apply -f configmap.yaml -n argocd
kubectl patch deployment argocd-repo-server -n argocd --patch-file patch-deployment.yaml

kubectl cluster-info

# JWT Token
kubectl exec argocd-repo-server-55667578cd-5zwcr --container argocd-repo-server -n argocd -- cat /var/run/secrets/kubernetes.io/serviceaccount/token
kubectl exec argocd-repo-server-55667578cd-5zwcr --container argocd-repo-server -n argocd -- cat /var/run/secrets/kubernetes.io/serviceaccount/ca.crt

Next steps can also be done via vault ui:

vault auth enable kubernetes

vault write auth/kubernetes/config \
  token_reviewer_jwt="<your reviewer service account JWT>" \
  kubernetes_host=https://192.168.99.100:<your TCP port or blank for 443> \
  kubernetes_ca_cert=@ca.crt


vault policy write argocd - <<EOF
# Allow reading secrets for your applications
path "secret/data/*" {
  capabilities = ["read", "list"]
}
path "secret/metadata/*" {
  capabilities = ["read", "list"]
}
# Add other paths as needed
EOF


vault write auth/kubernetes/role/argocd \
  bound_service_account_names=argocd-repo-server \
  bound_service_account_namespaces=argocd \
  policies=argocd \
  audience=argocd-repo-server \
  ttl=1h