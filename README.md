# Application Autoscaling with KEDA by listening Azure Service Bus Message Queue/Topics 

<ol>
  <li>We need to install keda into the cluster by using Helm Chart</li>
  <a href="https://github.com/kedacore/charts">Keda Install Helm Chart</a>
  <li>We need to create Trigger Authentication Object to connect Service Bus</li>
  <a href="https://keda.sh/docs/2.1/scalers/azure-service-bus/">KEDA - Azure Service Bus</a>
  <li>We need to create deployment yaml for the application with service bus connection string env variable</li>
  <li>We need to create a secret to keep connection string as encrypted</li>
</ol>
