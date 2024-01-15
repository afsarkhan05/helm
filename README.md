# helm
Helm related stuff for all other repo examples

## Run Minikube kubernetes Services using helm

* Minikube Cluster
* Docker
* Kubectl
* Helm


Make sure you logged to docker hub as below

```shell
docker login
```

Look for my docker hub repository at https://hub.docker.com/u/afsarkhan05

```
Alternatively, you can build the project at https://github.com/afsarkhan05/spring-projects
and create your own image with version and upload to your own docker hub repository
```

Use the latest and correct version of image and docker repo location and update in respective values files.

values-dev-eureka-server.yaml

```yaml
dockerImage:
  image: afsarkhan05/spring-discovery-service
  version: 1.0-SNAPSHOT
```

values-dev-spring-discovery-api-service.yaml

```yaml
dockerImage:
  image: afsarkhan05/spring-discovery-api-service
  version: 1.0-SNAPSHOT
```

Create persistent volume (Required for later project stuff)

```shell
  cd charts/spring-discovery/config
  kubectl apply -f persistentVolume.yaml
```

Start eureka server using helm

```shell
cd charts
helm install eureka-server-release -f spring-discovery/values-dev-eureka-server.yaml --create-namespace --namespace spring-projects ./spring-discovery
```


Start a spring boot service and register to eureka server

```shell
 helm install discovery-api-release -f spring-discovery/values-dev-spring-discovery-api-service.yaml --create-namespace --namespace spring-projects ./spring-discovery
```

As both the services are on ClusterIP hence to access them on local environment,
Forward ports as given below

```shell
kubectl port-forward service/eureka-server-service 8761:8761 -n spring-projects
```
access eureka server at http://localhost:8761/

```shell
kubectl port-forward service/spring-discovery-api-service 8081:8081 -n spring-projects
```

access the service at http://localhost:8081/first-service/hello


List down all the resources 

```shell
helm list -a -n spring-projects
```

delete the resource

```shell
helm delete -n spring-projects discovery-api-release
```

```shell
helm delete -n spring-projects eureka-server-release
```