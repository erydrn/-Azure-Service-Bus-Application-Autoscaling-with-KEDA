# -Azure-Service-Bus-Application-Autoscaling-with-KEDA

1 - We need to install keda into the cluster by using Helm Chart
https://github.com/kedacore/charts

https://keda.sh/docs/2.1/scalers/azure-service-bus/
2- We need to create Trigger Authentication Object to connect Service Bus
3- We need to create deployment yaml for the application with service bus connection string env variable
4- We need to create a secret to keep connection string as encrypted
