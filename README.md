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
