# Installation guide of 3 nodes opensearch-cluster

OpenSearch can operate as a single-node or multi-node cluster. Production setup typically requires a multi-node cluster. This will setup a three-node cluster that has one dedicated master node, one dedicated coordinating node, and one data node that are used for ingesting data. 

Also at end this guide will help to setup opensearch-dashboard as well if needed to expose via Ingress controller.
## Prerequisite
kubernetes cluster (1.19+)
, Worker nodes having atleast a minimum of 4GiB of memory is required. I would recommend to have 8 GiB of memory available for this setup to avoid any intermittent failures, Ingress configured and Ingress received IP via LoadBalancer as a service.

## Installation



```bash
cd opensearch/
kubectl apply -f opensearch-manifests/client-deployment.yaml
kubectl apply -f opensearch-manifests/data-deployment.yaml
kubectl apply -f opensearch-manifests/master-deployment.yaml
```
Once deployment is completed it should show 3 stateful-set pods are created.

```bash
kubectl get pods 
```
Now, setting up opensearch-dashboard deployment: (2)
```bash
kubectl apply -f opensearch-manifests/dashboard-deployment.yaml
kubectl get pods
```
To expose opensearch-dashboard service via ingress with URL http://opensearch-dashboard.example.com (If proper DNS entries are provided): (3)

```bash
kubectl apply -f ingress-service-manifests/opensearch-dashboard.yaml
kubectl get ingress
echo "<replace-with-ingress-ip> opensearch-dashboard.example.com" >> /etc/hosts
curl http://opensearch-dashboard.example.com
```

If not interested to use opensearch-dashboard avoid doing point 2,3 and follow below, this should expose opensearch-master via ingress service: 

```bash
kubectl apply -f ingress-service-manifests/opensearch-cluster-master.yaml
kubectl get ingress
echo "<replace-with-ingress-ip> opensearch-master.example.com" >> /etc/hosts
curl http://opensearch-master.example.com