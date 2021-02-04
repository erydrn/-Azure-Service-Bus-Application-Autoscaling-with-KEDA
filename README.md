# Application Autoscaling with KEDA by listening Azure Service Bus Message Queue/Topics 

<h3></h3>

<ol>
<li>
Install KEDA and add to our helm chart repo
<br>
<a href="https://github.com/kedacore/charts">Keda Install Helm Chart</a>
<pre>
<code style="color:black">$ helm repo add kedacore https://kedacore.github.io/charts</code>
"kedacore" has been added to your repositories
</pre>
<pre>
$ helm search repo kedacore/
NAME            CHART VERSION   APP VERSION     DESCRIPTION
kedacore/keda	2.0.1        	2.0.0      	Event-based autoscaler for workloads on Kubernetes
</pre>
</li>
<li>We need to create Trigger Authentication Object to connect Service Bus</li>
<a href="https://keda.sh/docs/2.1/scalers/azure-service-bus/">KEDA - Azure Service Bus</a>
<li>We need to create deployment yaml for the application with service bus connection string env variable</li>
<li>We need to create a secret to keep connection string as encrypted</li>
</ol>
