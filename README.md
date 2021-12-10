# webinar-airflow-chart
Materials of the Official Helm Chart Webinar
# Installation
1. docker, docker-compose
2. kind (K8s on Docker)
3. kubectl
4. helm

# 1. Kubernetes Deploy cluster 
Using KinD and configure via *kind-cluster.yaml*

<code>kind create cluster --name airflow-cluster --config=kind-cluster.yaml</code>

Check cluster info
1. `kubectl cluster-info`
2. `kubectl node ls`

Create namespace airflow
`kubectl create namespace airflow`

`kubectl get ns`

# 2. Helm Chart Deploy Airflow
1. `helm repo add apache-airflow https://airflow.apache.org
2. `helm repo update` (Get latest version of helm chart)
3. `helm search repo airflow` (list of helm charts)

5. Deploy airflow  
  `helm install airflow apache-airflow/airflow --namespace airflow --debug --timeout 10m0s` (App. name = airflow, Repo=apache-airflow/airflow, k8s-namespace=airflow)   
6. `helm ls -n airflow`
7. By default CeleryExecutor  
    `kubectl get pods -n airflow`
8. `kubectl logs -f deployment/airflow-scheduler -n airflow`
9. To check UI, `kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow`

# 3. Upgrade Airflow
