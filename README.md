# install ArgoCD in k8s
Commands:
`kubectl create namespace argocd`
`kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

# access ArgoCD UI
Commands:
`kubectl get svc -n argocd`
`kubectl port-forward svc/argocd-server 8080:443 -n argocd`

# login with "admin" user and below token (as in documentation):
Command:  `argocd admin initial-password -n argocd`
(make sure to update new password in the User Info option.)

# Ingress setup on minikube

To manage ingress controllers, Kubernetes provides `Nginx Ingress Controller`, other third party tools like Istio and Linkerd.can also be used for this. Nginx Ingress Controller's support is extended as addons in minikube as well .
With a simple command all dependencies will be made available and ingress will be enabled.

Command:  `minikube addons enable ingress`

Output reference:

<img width="822" alt="image" src="https://user-images.githubusercontent.com/60884268/224271075-80461663-260a-4723-aff3-a9bee83c5f0e.png">

Once ingress addon is enable a new namespace "ingress-nginx" is created in minikube. You can verify the status of all resources created in ingress-nginx namespace using below command:

Command: kubectl get all -n ingress-nginx

Output reference:

<img width="892" alt="image" src="https://user-images.githubusercontent.com/60884268/224272830-90d5e9df-d3d5-4c34-8abd-9b44cc1f017c.png">

# Argo Rollouts installation

`kubectl create namespace argo-rollouts`
`kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml`

Note: Argo Rollouts CRDs are not included into namespace-install.yaml. and have to be installed separately. The CRD manifests are located in manifests/crds directory. Use the following command to install them:

`kubectl apply -k https://github.com/argoproj/argo-rollouts/manifests/crds\?ref\=stable`

# Install Argo Rollouts Kubectl plugin with curl.

For Linux dist, replace darwin with linux

`curl -LO https://github.com/argoproj/argo-rollouts/releases/latest/download/kubectl-argo-rollouts-darwin-amd64`
`chmod +x ./kubectl-argo-rollouts-darwin-amd64`
`sudo mv ./kubectl-argo-rollouts-darwin-amd64 /usr/local/bin/kubectl-argo-rollouts`

Test to ensure the version you installed is up-to-date:
`kubectl argo rollouts version`

#Links for reference

[Argo Rollout Installation](https://argoproj.github.io/argo-rollouts/installation/)

Steps
kubectl port-forward svc/argocd-server 8080:443 -n argocd
minikube service realtimeapp-active -n realtime-rollout

Check revision history

kubectl-argo-rollouts get rollout realtimeapp -n realtime-rollout

<img width="719" alt="image" src="https://user-images.githubusercontent.com/60884268/225228584-e68358f3-64ac-4317-8b31-472e58eac2b6.png">

Argo Rollouts Dashboard

kubectl argo rollouts dashboard -n realtime-rollout

<img width="956" alt="image" src="https://user-images.githubusercontent.com/60884268/225229391-cdcfff57-9588-4427-a900-69ae1322e09b.png">

Promote new image to active service

kubectl argo rollouts promote realtimeapp -n realtime-rollout



