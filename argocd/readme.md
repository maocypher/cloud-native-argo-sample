https://github.com/argoproj/argo-helm
helm repo add argo https://argoproj.github.io/argo-helm

helm template argocd \
  oci://ghcr.io/argoproj/argo-helm/argo-cd \
  --api-versions monitoring.coreos.com/v1 \
  --values override-values.yaml

helm install argocd argo/argocd --values override-values.yaml -n argocd