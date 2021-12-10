# webinar-airflow-chart
Materials of the Official Helm Chart Webinar
# Installation
1. docker, docker-compose
2. kind (K8s on Docker)
3. kubectl
4. helm

# Kubernetes Deploy cluster 
Using KinD and configure via *kind-cluster.yaml*

<code>kind create cluster --name airflow-cluster --config=kind-cluster.yaml</code>

Check cluster info
1. `kubectl cluster-info`
2. `kubectl node ls`

Create namespace airflow
`kubectl create namespace airflow`

`kubectl get ns`
