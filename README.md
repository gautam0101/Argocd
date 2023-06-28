# Amazon EKS and ArgoCD
Argo CD is a declarative continuous delivery tool for Kubernetes applications. It uses the GitOps style to create and manage Kubernetes clusters. When any changes are made to the application configuration in Git, Argo CD will compare it with the configurations of the running application and notify users to bring the desired and live state into sync.

Argo CD has been developed under the Cloud Native Computing Foundation’s (CNCF) Argo Project- a project, especially for Kubernetes application lifecycle management. The project also includes Argo Workflow, Argo Rollouts, and Argo Events.. Each solves a particular set of problems in the agile development process and make the Kubernetes application delivery scalable and secure.

video:- [Video](https://drive.google.com/file/d/1UIwsEz_pLyWRgBqc3X_Q-WJAWfds30Vy/view?usp=sharing)

### Upgrade Packages & Install Prerequisites

1. AWS cli.
2. AWS ecr repo

![Screenshot from 2023-06-12 13-21-10](https://github.com/gautam0101/argocd/assets/101164301/a5339399-2e0f-41f6-ae2a-fb60d1668c76)


### Create AWS EKS cluster

 `aws sts get-caller-identity`
 
 `eksctl create cluster --name <Your Cluster_Name> --region <Your_AWS_Region>`
 
 `aws eks update-kubeconfig --region <Your_AWS_Region> --name <Your Cluster_Name>`
 
 `kubectl cluster-info`
 
 ![Screenshot from 2023-06-12 13-21-22](https://github.com/gautam0101/argocd/assets/101164301/b1ceaabf-acf6-4195-8702-5b2a92c13bcb)


### Install ArgoCD
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
![Screenshot from 2023-06-13 13-27-07](https://github.com/gautam0101/argocd/assets/101164301/e21df28c-2c9c-42a5-93fe-15a11ede0b93)


### Fetch Password
```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
![Screenshot from 2023-06-13 13-26-50](https://github.com/gautam0101/argocd/assets/101164301/77d1d87a-bb5d-4e2f-a064-4f85b356dc57)

### How to open argocd in local

check all pods are running:- `kubectl get pods -n argocd`

for open argocd in 8080 port:- `kubectl port-forward svc/argocd-server -n argocd 8080:443`

![Screenshot from 2023-06-13 13-43-48](https://github.com/gautam0101/argocd/assets/101164301/c4865920-22d2-49a4-b9fa-e6437206c620)

### Create application in argo cd

![Screenshot from 2023-06-13 13-45-00](https://github.com/gautam0101/argocd/assets/101164301/51d99469-b0f5-4127-b301-545e966acdcd)

Once the EKS cluster created and configue the ArgoCD on cluster, let’s create application on ArgoCD.

![Screenshot from 2023-06-13 13-45-13](https://github.com/gautam0101/argocd/assets/101164301/38fb0239-4455-4c5b-8c0e-7a401d7c3027)

Click create application on ArgoCD CLI. And set the following values for the application.

1. Application name : abcd
2. Project name : default
3. sync policy :- Autom
4. Reposatry url:- github url
5. path:- set the path charts/helm
6. cluster url:- https//kubernetes.default.svc
7. newspace:- default

Then click create. And you will be redirected into your newly created application on cluster.

![Screenshot from 2023-06-13 13-58-52](https://github.com/gautam0101/argocd/assets/101164301/0be75161-13d9-43dd-bf6c-1f55b84024f8)



