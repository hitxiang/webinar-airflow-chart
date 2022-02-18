# webinar-airflow-chart
Materials of the Official Helm Chart Webinar
All the tips are based marclamberti's great work when I learned his course on udemy.
 please reference https://marclamberti.com/blog/airflow-on-kubernetes-get-started-in-10-mins/
# Installation
1. docker, docker-compose
2. kind (K8s on Docker)
3. kubectl
4. helm

# 1. Kubernetes Deploy cluster 
Using KinD and configure via *kind-cluster.yaml*

`kind create cluster --name airflow-cluster --config=kind-cluster.yaml`

Check cluster info
1. `kubectl cluster-info`
2. `kubectl get nodes -o wide`

Create namespace airflow
```
kubectl create namespace airflow

kubectl get ns
```

# 2. Helm Chart Deploy Airflow
1. `helm repo add apache-airflow https://airflow.apache.org`
2. `helm repo update` (Get latest version of helm chart)
3. `helm search repo airflow` (list of helm charts)

5. Deploy airflow  
  ```
  helm -n airflow upgrade --install airflow apache-airflow/airflow --debug
  helm -n airflow history  airflow 
  #  helm -n airflow uninstall airflow
  ```
  
   (release_name = airflow, Repo=apache-airflow/airflow) 
 
6. `helm ls -n airflow`


7. By default CeleryExecutor  
    `kubectl get pods -n airflow`

add more memory to docker when encontuer `Unable to connect to the server: net/http: TLS handshake timeout` error

8. `kubectl logs -f deployment/airflow-scheduler -n airflow`
9. To check UI
```
kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow
```

# 3. Upgrade Airflow
1. helm show values apache-airflow/airflow > original_values.yaml
2. Make changes in values.yaml for airflow version, scheduler(KubernetesExecutor)
```
executor: "KubernetesExecutor"
```
3. `helm upgrade --install  airflow apache-airflow/airflow -n airflow -f values_v1.yaml --debug`
```
helm upgrade -f new-values.yml {release name} {package name or path} --version {fixed-version}
```

# 4. Use customized docker image
```
docker build -t airflow-custom:latest .
docker tag airflow-custom:latest airflow-custom:1.0.0

kind --name airflow-cluster load docker-image airflow-custom:1.0.0 

helm upgrade --install  airflow apache-airflow/airflow -n airflow -f values.yaml --debug
kubectl -n airflow get pods

# confirm the package in requirements.txt are installed
kubectl -n airflow exec -it airflow-webserver-77565d76bd-cx2zh -- airflow info | grep great-expectations 
airflow-provider-great-expectations      | 0.0.6

kubectl exec <webserver_pod_id> -n airflow -- airflow providers list | grep great-expectations 

```

# 5. Logs with Airflow on Kubernetes
```
kubectl apply -f pv.yaml
kubectl get pv -n airflow

kubectl apply -f pvc.yaml 
kubectl get pvc -n airflow
```

updating the following in values.yml

```
logs:   
  persistence:     
    enabled: true     
    existingClaim: airflow-logs

# enable samples
config:
  core:
    load_examples: 'True'
```

Redeploy
`helm upgrade --install  airflow apache-airflow/airflow -n airflow -f values.yaml --debug`

