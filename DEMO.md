
# 1. Minikube
brew install minikube
minikube start --cpus=2 --memory=4096

# 2. ArgoCD (if not installed)
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# 3. Admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# 4. Port-forward
kubectl port-forward svc/argocd-server -n argocd 8080:443

# 5. Create app
argocd app create demo-app \
  --repo https://github.com/murgovskixm/gitops-demo \
  --path app \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default

# Cleanup
kubectl delete -n argocd application demo-app
minikube stop
