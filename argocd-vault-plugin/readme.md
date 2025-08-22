kubectl apply -f configmap.yaml -n argocd
kubectl patch deployment argocd-repo-server -n argocd --patch-file argocd-repo-server-patch.yaml
kubectl apply -f service-account.yaml -n argocd
kubectl patch deployment argocd-repo-server -n argocd -p='{"spec":{"template":{"spec":{"serviceAccountName":"argocd-vault-plugin"}}}}'