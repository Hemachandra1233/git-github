# argocd-infra
Infra GitOps managed by ArgoCD

# For ingress networking use docker-desktop instead of minikube

# Setup minikube cluster for k8s
<ol>
<li>Download minikube from https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe </li>
<li>Rename to minikube.exe</li>
<li>Add minikube.exe to PATH variable</li>
</ol>

```
minikube version
minikube status
```

# Setup kubectl
<ol>
<li>Download kubectl from https://dl.k8s.io/release/v1.25.0/bin/windows/amd64/kubectl.exe </li>
<li>Add kubectl.exe to PATH variable</li>
</ol>

```
kubectl version
```

# Setup Helm
<ol>
<li>Download helm from https://get.helm.sh/helm-v3.10.2-windows-amd64.zip </li>
<li>Extract windows-amd64\helm.exe from helm-v3.10.2-windows-amd64.zip</li>
<li>Add helm.exe to PATH variable</li>
</ol>

```
helm version
```

# Setup ArgoCD tool

Verify your minikube or k8s cluster

```
minikube status
kubectl config use-context minikube
kubectl get pods -A
```

Deploy ArgoCD

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd
```

Download argocd CLI
<ol>
<li>Download executable from https://github.com/argoproj/argo-cd/releases/download/v2.5.2/argocd-windows-amd64.exe</li>
<li>Rename to argocd.exe</li>
<li>Add argocd.exe to PATH variable</li>
</ol>

## Get ArgoCD Admin Password

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

```
## Expose ArgoCD Server

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
## Access ArgoCD from CLI
```
argocd login https://localhost:8080/ --username admin --password ${ARGOCD_PASSWORD}
```
## Access ArgoCD From UI
Open https://localhost:8080 with credentials admin/${ARGOCD_PASSWORD}

# Enable self-management with GitOps
```
argocd login https://localhost:8080/ --username admin --password ${ARGOCD_PASSWORD}
argocd repo add --name argocd-gitops git@github.com:hakella10/gitops.git --ssh-private-key-path C:\Users\HariAkella\.ssh\id_rsa
kubectl config use-context minikube
kubectl apply -f cluster-bootstrapper.yaml
```

Deploy cert-manager , ingress-nginx controller and argocd-ingress to make the ArgoCD UI avaiable without port forwarding
