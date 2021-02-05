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
 ### Create secret object in AKS and set Service Bus Topic/Queue Connection String
 
 ![Image of Service Bus Topic Portal](https://github.com/erydrn/Azure-Service-Bus-Application-Autoscaling-with-KEDA/blob/main/images/ServiceBusQueueConnString.png)
 
  ```
  apiVersion: v1
  kind: Secret
  metadata:
    name: order-secrets
    labels:
      app: order-processor
  data:
    SERVICEBUS_TOPIC_CONNECTIONSTRING: {Encoded Base64 Service Bus Topic/Queue Connection String}
  ```
  ```
  kubectl create -f secret.yaml
  ```
  ### Create Trigger Authentication Object to let KEDA access the Service Bus Queue/Topic with the Connection String defined in secret and pulled from environment variable in application deployment object
  ```
  apiVersion: keda.sh/v1alpha1
  kind: TriggerAuthentication
  metadata:
    name: trigger-auth-service-bus-orders
  spec:
    secretTargetRef:
    - parameter: connection
      name: order-secrets
      key: SERVICEBUS_TOPIC_CONNECTIONSTRING
  ```
  ### Create deployment object for the application in AKS
  ```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: order-processor
    labels:
      app: order-processor
  spec:
    selector:
      matchLabels:
        app: order-processor
    template:
      metadata:
        labels:
          app: order-processor
      spec:
        containers:
        - name: order-processor
          image: nginx
          imagePullPolicy: Always
          env:
          - name: KEDA_SERVICEBUS_TOPIC_CONNECTIONSTRING
            valueFrom:
              secretKeyRef:
                name: order-secrets
                key: SERVICEBUS_TOPIC_CONNECTIONSTRING
   ```
   ### Create scaled object to set autoscaling rules for KEDA in AKS
   ```
    apiVersion: keda.sh/v1alpha1
    kind: ScaledObject
    metadata:
      name: order-processor-scaler
      labels:
        app: order-processor
        deploymentName: order-processor
    spec:
      scaleTargetRef:
        name: order-processor
      minReplicaCount: 0
      cooldownPeriod: 30
      pollingInterval: 5
      maxReplicaCount: 10
      triggers:
      - type: azure-servicebus
        authenticationRef:
          name: trigger-auth-service-bus-orders
        metadata:
          queueName: queueone
          #topicName: orders
          #subscriptionName: sbtopic-sub1
          connection: KEDA_SERVICEBUS_TOPIC_CONNECTIONSTRING
          queueLength: '5'
