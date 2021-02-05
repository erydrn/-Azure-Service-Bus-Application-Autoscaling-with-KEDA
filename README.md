# Application Autoscaling with KEDA by listening Azure Service Bus Message Queue/Topics 

## Getting Started
### Adding our Helm chart repo

```console
$ helm repo add kedacore https://kedacore.github.io/charts
"kedacore" has been added to your repositories
```
You can find the latest releases [here](https://github.com/kedacore/charts/releases)

### Browse all our Helm charts
```
$ helm search repo kedacore/
NAME            CHART VERSION   APP VERSION     DESCRIPTION
kedacore/keda	2.0.1        	2.0.0      	Event-based autoscaler for workloads on Kubernetes
```
### Change values.yaml
Change values in values.yaml based on your configurations. I have changed the version of the image, and set resource limits

```
image:
  keda:
    repository: docker.io/kedacore/keda
    # Allows people to override tag if they don't want to use the app version
    tag: 2.1.0
  metricsApiServer:
    repository: docker.io/kedacore/keda-metrics-apiserver
    # Allows people to override tag if they don't want to use the app version
    tag: 2.1.0
  pullPolicy: Always
```
```
resources: 
   limits:
     memory: 256Mi
   requests:
     memory: 256Mi
 ```
 ### Create namespace in AKS and install the helm chart
 ```
 kubectl create ns keda
 helm install keda kedacore/keda -n keda -f values.yaml
  ```
