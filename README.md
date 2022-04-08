# Nextcloud Kubernetes
kubectl create namespace nextcloud

## Prototype
The original source is here: https://blog.true-kubernetes.com/self-host-nextcloud-using-kubernetes/ all the credits to the blog writer.

kubectl create secret generic nextcloud-db-secret \  
    --from-literal=MYSQL_ROOT_PASSWORD=rootPassw0rd1 \
    --from-literal=MYSQL_USER=nextcloud \
    --from-literal=MYSQL_PASSWORD=passw0rd1

## Manual Setup
Diffwerence to the original prototype is the use of Postgersql as the database.

kubectl create secret generic nextcloud-db-secret --from-literal=POSTGRES_USER=nextcloud --from-literal=POSTGRES_PASSWORD=passw0rd1

The persistent volume and persistent volume claim of both the data bse and the srver have been decoupled due to issues on the restart of the DB, persumably due to different permissions set by the applications.
Caveat: the host path persistent volume of the server had to be created by hand into the host, to be reviewed.

### SSL
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.yaml

#### References
https://someweb.github.io/devops/cert-manager-kubernetes/
https://cert-manager.io/docs/installation/kubectl/
https://cert-manager.io/docs/configuration/selfsigned/

## Helm Setup
helm repo add nextcloud https://nextcloud.github.io/helm/
helm repo update
helm show values nextcloud/nextcloud >> nextcloud.values.yml

helm install nextcloud nextcloud/nextcloud --namespace nextcloud --values nextcloud.values.yml