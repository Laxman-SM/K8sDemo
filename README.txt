
Steps to reproduce the environment creation. 

1. Manual Creation
2. Script creation.

az account set --subscription <subid>
az account list --output table

az group create --subscription <subid> --location westeurope --name AksRG

az aks get-versions --location westeurope

1.19.0

az aks create --resource-group AksRG --name PayCls --node-count 2 --enable-addons monitoring --kubernetes-version 1.19.0 --generate-ssh-keys

az aks get-credentials --resource-group AksRG --name PayCls

kubectl config get-contexts

CURRENT   NAME             CLUSTER          AUTHINFO                               NAMESPACE
*         PayCls           PayCls           clusterUser_AksRG_PayCls

kubectl get all --all-namespaces

kubectl get nodes

Mysql Helm Chart and log enablement

kubectl get pv,pvc --all-namespaces

kubectl  get StorageClass

kubectl apply -f db-storage.yml
storageclass.storage.k8s.io/managed-premium-retain created



kubectl apply -f db-storage.yml
storageclass.storage.k8s.io/managed-premium-retain configured

kubectl create -f db-pvc.yml
persistentvolumeclaim/payedb created


helm lint==> Linting .

1 chart(s) linted, 0 chart(s) failed

helm install --generate-name --set mysql.persistence.enabled=true,persistence.storageClass=managed-premium,mysql.persistence.existingClaim=payedb .

helm upgrade --install chart-1602563249 --set mysql.persistence.enabled=true,persistence.storageClass=managed-premium,mysql.persistence.existingClaim=payedb .

----------------------------------------------------------------------------------------------------------------------------

helm repo add stable https://kubernetes-charts.storage.googleapis.com/

helm repo update

kubectl create namespace monitoring

sh  install_prometheus.sh

helm upgrade --install prometheus --namespace monitoring stable/prometheus-operator --values ../helm/prometheus_values.yaml --values ../helm/alertmanager_values.yaml

kubectl --namespace monitoring get pods -l "release=prometheus"

kubectl describe svc prometheus-prometheus-oper-prometheus -n monitoring

kubectl edit svc prometheus-prometheus-oper-prometheus -n monitoring

http://x.x.x.x:9090/graph

----------------------------------------------------------------------------------------------------------------------------------

1. helm install payegra grafana/grafana --namespace monitoring --set rbac.create=false --set service.type=LoadBalancer --set persistence.enabled=true

2. The Grafana server can be accessed via port 80 on the following DNS name from within your cluster:

3. Login with the password from step 1 and the username: admin

Add Datasource prometheus

http://x.x.x.x/login

Import Dashboard 

select data Source.

save
==================================================================================================================================
