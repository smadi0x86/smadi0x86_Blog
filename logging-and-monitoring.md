# Logging & Monitoring

## Monitor Cluster Components

Metrics server is a monitoring solution for Kubernetes, every Kubernetes cluster can have only one metrics server, it collects metrics from nodes and pods and places them In-memory (not on disk).

Kubelet has a sub-component called cAdvisor and its responsible for retrieving performance metrics from pods and expose them through kubelet API so they can be used by metrics server.

#### You can download metrics server by running:

{% code overflow="wrap" %}
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
{% endcode %}

#### After downloading metrics server, view CPU and Memory consumption of nodes and pods:

```bash
kubectl top node
kubectl top pod
```

### Lab:

No issues identified.

## Managing Application Logs

#### We can create a logging pod using a definition yaml:

#### As we create the pod, we can view logs:

```bash
kubectl logs -f event-simulator-pod image-processor # -f for realtime
```

### Lab:

No issues identified.

## Slides
