Argo CD Minikube Demo

Minimal steps to spin up from scratch on a fresh Minikube.

Prerequisites
- minikube
- kubectl
- helm

1) Start Minikube
```bash
minikube start
```

2) Install Argo CD via Helm (5s reconciliation preconfigured)
```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
kubectl create namespace argocd
helm upgrade --install argocd argo/argo-cd -n argocd -f argocd/helm/values.yaml
```

Optional: access Argo CD UI
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
# open https://localhost:8080 (admin / initial password from secret)
```


4) Bootstrap via app-of-apps
```bash
kubectl apply -n argocd -f argocd/root-app.yaml
```

5) Access demo app(s)
```bash
minikube service demo-web-v1 -n demo --url
minikube service demo-web-v2 -n demo --url
```


Cleanup (optional)
```bash
kubectl delete -n argocd -f argocd/root-app.yaml || true
kubectl delete ns demo || true
kubectl delete ns argocd || true
```

