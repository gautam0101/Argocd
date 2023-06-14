# Amazon EKS and ArgoCD
Argo CD is a declarative continuous delivery tool for Kubernetes applications. It uses the GitOps style to create and manage Kubernetes clusters. When any changes are made to the application configuration in Git, Argo CD will compare it with the configurations of the running application and notify users to bring the desired and live state into sync.

Argo CD has been developed under the Cloud Native Computing Foundation’s (CNCF) Argo Project- a project, especially for Kubernetes application lifecycle management. The project also includes Argo Workflow, Argo Rollouts, and Argo Events.. Each solves a particular set of problems in the agile development process and make the Kubernetes application delivery scalable and secure.

video:- [Video]()

### Upgrade Packages & Install Prerequisites

1. AWS cli.

### Create AWS EKS cluster

 `aws sts get-caller-identity`
 `eksctl create cluster --name <Your Cluster_Name> --region <Your_AWS_Region>`
 `aws eks update-kubeconfig --region <Your_AWS_Region> --name <Your Cluster_Name>`
 `kubectl cluster-info`

### Install ArgoCD
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
### Change Service to NodePort
Edit the service can change the service type from `ClusterIP` to `NodePort`
```sh
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}' 
```
### Fetch Password
```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
# Deploy Demo Application
You can use the below repository to deploy a demo nginx application
```sh
https://github.com/dmancloud/argocd-tutorial
```
### Scale Replicaset 
```sh
kubectl scale --replicas=3 deployment nginx -n default
```
### Clean Up
```sh
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl delete namespace argocd
```
