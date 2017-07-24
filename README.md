# ZOP Demo

## Installation

Before we begin, you will need to have an existing Kubernetes cluster up and running with a decent amount of memory and CPU on each Kubernetes worker node. If you need to stand up a cluster, there is documentation available on the official Kubernetes site for [setup](https://kubernetes.io/docs/setup/). It is assumed that you have a good understanding and grasp at operating/navigating a Kubernetes cluster.

To simplify and condense this walkthrough, you can find all configuration files this GitHub repo.

### Deploying Zipkin
<pre>
# We need to open up the Zipkin port to access the UI.
# If you are behind a firewall whether it's on-prem or in your
# favorite cloud like GCE, don't forget to open up the NodePort
# that is allocated!
cd services
kubectl create -f zipkin.yaml
cd ..

# Let's deploy Zipkin
cd deployments
kubectl create -f zipkin.yaml
cd ..
</pre>

### Deploying Prometheus
<pre>
# Save the configuration file in Kubernetes for use by Prometheus
cd configs
kubectl create configmap prometheus --from-file=config.yaml --namespace=kube-system
cd ..

# We need to open up the Prometheus port to access the UI.
# If you are behind a firewall whether it's on-prem or in your
# favorite cloud like GCE, don't forget to open up the NodePort
# that is allocated!
cd services
kubectl create -f prometheus.yaml
cd ..

# Let's deploy Prometheus
cd deployments
kubectl create -f prometheus-scratch.yaml
cd ..
</pre>

### Deploying the Sample Application!
<pre>
# We need to open up the Sample Application port to access the Front UI.
# If you are behind a firewall whether it's on-prem or in your
# favorite cloud like GCE, don't forget to open up the NodePort
# that is allocated!
cd services
kubectl create -f backend.yaml
kubectl create -f frontend.yaml
cd ..

# Let's deploy the Sample Application
cd deployments
kubectl create -f backend.yaml
kubectl create -f frontend.yaml
cd ..

# Let's hit the frontend service using curl!
curl http://<PUBLIC_IP_ADDRESS>:<NodePort>
</pre>

## Teardown

Run the following commands to remove the ZOP stack from your Kubernetes cluster.
<pre>
kubectl delete deployment frontend
kubectl delete deployment backend
kubectl delete deployment prometheus --namespace=kube-system
kubectl delete deployment zipkin --namespace=kube-system
kubectl delete service frontend
kubectl delete service backend
kubectl delete service prometheus --namespace=kube-system
kubectl delete service zipkin --namespace=kube-system
kubectl delete configmap prometheus --namespace=kube-system
</pre>
