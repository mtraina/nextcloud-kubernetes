# Nextcloud Kubernetes
## Setup
kubectl create namespace nextcloud

helm repo add nextcloud https://nextcloud.github.io/helm/
helm repo update
helm show values nextcloud/nextcloud >> nextcloud.values.yml

helm install nextcloud nextcloud/nextcloud --namespace nextcloud --values nextcloud.values.yml