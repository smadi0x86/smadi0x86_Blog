# Networking

## Prerequisites

### Network Namespaces

A network namespace is like a separate room for network devices and configurations on a computer, where each namespace is isolated from others.

Containers typically use network namespaces to keep their network activities separate from the rest of the system.

**Add Namespaces**:

* Create a namespace 'red':

```bash
ip netns add red
```

* Create another namespace 'blue':

```bash
ip netns add blue
```

**List Namespaces**:

```bash
ip netns
```

**Create a Virtual Cable (veth pair)**:

```bash
ip link add veth-red type veth peer name veth-blue
```

**Attach Each End to a Namespace**:

* Attach `veth-red` to 'red':

```bash
ip link set veth-red netns red
```

* Attach `veth-blue` to 'blue':

```bash
ip link set veth-blue netns blue
```

#### Assigning IP Addresses

* Set IP for 'red' namespace:

```bash
ip -n red addr add 192.168.15.1/24 dev veth-red
```

* Set IP for 'blue' namespace:

```bash
ip -n blue addr add 192.168.15.2/24 dev veth-blue
```

#### Activating the Network Interfaces

* Turn on interface in 'red':

```bash
ip -n red link set veth-red up
```

* Turn on interface in 'blue':

```bash
ip -n blue link set veth-blue up
```

{% hint style="info" %}
Ping to check connectivity between namespaces.
{% endhint %}

#### When network namespaces increase, its not efficient to do previous steps for each one, so we use Linux bridge to connect multiple network namespaces

**Create a Bridge**:

```bash
sudo ip link add name br0 type bridge
```

**Activate the Bridge**:

```bash
sudo ip link set br0 up
```

#### Connecting Namespaces to the Bridge:

```bash
ip link add veth1 type veth peer name veth1-br
```

**Connect Ends to Namespace and Bridge**:

* Attach to namespace:

```bash
ip link set veth1 netns [namespace]
```

* Attach to bridge:

```bash
sudo ip link set veth1-br master br0
```

**Activate Both Ends**:

* In namespace:

```bash
sudo ip netns exec [namespace] ip link set veth1 up
```

* On bridge:

```bash
sudo ip link set veth1-br up
```

#### Enabling Communication from the Host to Namespaces:

```bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

**Optional NAT Setup (For External Access)**:

```bash
sudo iptables -t nat -A POSTROUTING -s [Bridge Network CIDR] ! -o br0 -j MASQUERADE
```

### Docker Networking

#### Docker Network Types

* **Bridge**: Default network for isolated communication between containers.
* **Host**: Removes network isolation, containers share host's network.
* **Overlay**: Enables networking across multiple Docker hosts, used in Docker Swarm.
* **Macvlan**: Assigns MAC addresses to containers, making them appear as physical devices.

#### Key Commands

* `docker network ls`: List networks.
* `docker network create`: Create a network.
* `docker network rm`: Remove a network.
* `docker network connect`: Connect a container to a network.
* `docker network disconnect`: Disconnect a container from a network.
* `docker network inspect`: Display detailed network information.
* `docker run -p [HOST_PORT]:[CONTAINER_PORT]`: Map container port to host.

#### Important Concepts

* Containers in the same network can communicate.
* Port mapping is needed for external access in bridge networks.
* Docker has a built-in DNS server for service discovery in user-defined networks.

### CNI

CNI (Container Network Interface) is a standard for configuring network interfaces for Linux containers.

CNI uses plugins to add or remove network interfaces from containers.

Provides a uniform networking setup across different container runtimes.

#### How CNI Works

1. **Creation**: Containers start with no network (`--network=none` in Docker).
2. **Networking**: CNI plugins set up network namespaces and attach interfaces, like virtual Ethernet (vEth) pairs, to them.
3. **Connectivity**: Plugins connect one end of a vEth pair to the container and another to the host bridge, enabling communication.
4. **IP Assignment**: CNI plugins assign IP addresses, set up routing, and manage IP masquerading for external access.

#### CNI with Container Runtimes

* **Docker**: Can use CNI through third-party plugins.
* **Kubernetes**: Natively uses CNI to manage pod networking—starts pods without network, and CNI configures the rest.

## Cluster Networking

Each node must have at least one interface connected to a common network. Each interface must have an IP address configured. Every node must have a unique hostname as well as a unique MAC address.

Various ports need to be opened (firewall and security group settings must be configured) on the master and worker nodes as shown in the diagram. The worker nodes expose services for external access on ports 30000 - 32767 (for `NodePort` services).

{% hint style="info" %}
In a multi-master setup, the ETCD clients need to communicate on port 2380, so that needs to be open as well.
{% endhint %}

{% embed url="https://kubernetes.io/docs/reference/networking/ports-and-protocols/" %}

{% hint style="warning" %}
In the official exam, all essential CNI deployment details will be provided.
{% endhint %}

### Lab (IMPORTANT):

This was so bad, I didn't answer well.

#### Here is the most useful stuff discovered:

* `ip link` and `ip address` commands are the same and deliver same information
* The `ip address/ip a` command displays information regarding mac address, the interface which is used by the IP address and the IP address itself.

#### If you want to view a single interface information run:

```bash
ip a show eth0
```

{% hint style="danger" %}
Don't forget to SSH to a worker node if asked to view its network information
{% endhint %}

#### If you want to view network bridges:

```bash
ip a show type bridge
```

#### To view routing and get the default gateway:

```bash
ip route
```

#### View port information:

```bash
netstat -nlp # use the -h to view more options
# -n options which means don't resolve names means that keep the port service name
```

```bash
netstat -anp | grep etcd # Check how many established connections are on etcd
```

{% hint style="info" %}
This showed how many established connections are on ETCD at port 2379

This is because all control plane components connect to ETCD at port 2379, 2380 and others are for peer to peer connectivity which is used when you have multiple master nodes.
{% endhint %}

## Pod Networking

#### Kubernetes defined requirements for pod networking which are:

{% hint style="success" %}
We must create a bridge on every node so pods can pair to the bridge and communicate with each other
{% endhint %}

#### When we create the bridge on a node, we must create a script to attach pods to the bridge:

* Create veth pair
* Attach veth pair
* Assign IP Address
* Bring Up Interface

{% hint style="warning" %}
There are other stuff such as configuring routing through a router and pointing all hosts to use it as the default gateway
{% endhint %}

#### To run this script automatically when a pod gets created, here is where CNI comes in to play and we must follow its standards:

{% hint style="info" %}
It must have an ADD section and a DELETE section
{% endhint %}

Now we configure the container runtime to look at the CNI configuration and identifies out the script name, which then looks in the bin directory to execute our script.

## CNI in Kubernetes

The CNI plugin must be invoked by the component within Kubernetes which is responsible for creating containers, this component must invoke the appropriate networking plugin after the container is created.

#### The CNI plugin is configured in the kubelet service on each node in the cluster:

#### You can view same information running:

```bash
ps -aux | grep kubelet
```

#### The CNI bin directory has all the supported CNI plugins as executables:

#### The CNI config directory has set of configuration files which the kubelet looks at to find which plugin to be used:

####

### Lab:

Focus on the paths of CNI.

#### View CNI plugins:

```bash
ls /opt/cni/bin
```

#### View CNI plugin configured to be used in current cluster:

```bash
ls /etc/cni/net.d
```

## CNI Weave

WeaveNet is a CNI compatible networking solution plugin for K8s. Instead of configuring a router to route traffic between nodes, it deploys an agent on each node. These agents communicate with each other to share information about their node. Each agent stores the entire network topology, so they know the pods and their IPs on the other nodes.

Weave creates a separate bridge network on each node called `weave` and connects the pods to it. The pods are connected to the docker bridge network and the weave bridge network at the same time. Weave configures the routing on the pods to ensure that all packets destined for other nodes are delivered to the agent. The agent then encapsulates the packet and sends it to the agent running on the other node, which then decapsulates the original packet and sends it to the required pod.

Weave allocates the CIDR `10.32.0.0/12` for the entire cluster. This allows for IP range `10.32.0.1` to `10.47.255.254` (over a million IPs). Weave peers (agents) split this range among themselves.

Weave can be deployed as a deamonset on each node. For installation steps, refer [Integrating Kubernetes via the Addon (weave.works)](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-installation) and [https://github.com/weaveworks/weave/releases](https://github.com/weaveworks/weave/releases)

#### Use the below latest link to install the **weave net**:

{% code overflow="wrap" %}
```bash
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```
{% endcode %}

Weave deploys a pod on each node of the Kubernetes cluster to manage the network.

#### The pod consists of two containers:

* Weave Net itself
* Weave Net NPC (Network Policy Controller), which manages network policies.

#### You can check the status of Weave Net pods just like any other pod in Kubernetes using:

```bash
kubectl get pods --all-namespaces -o wide
```

The output shows Weave Net pods are running on `master`, `node01`, `node02`, and `node03`, indicating that the network has been established across all these nodes.

```bash
kubectl logs weave-net-5gcmb -n kube-system
```

Fetches logs for troubleshooting and monitoring the Weave Net pod (`weave-net-5gcmb` in this case) within the `kube-system` namespace.

```bash
kubectl get daemonset weave-net -n kube-system
```

Checks the status of the Weave Net DaemonSet to ensure it's been deployed to all nodes.

### Lab:

To deploy a pod on a certain node, make sure to add the `nodeName` under the `spec:`

#### Then to check what this pod used as a default gateway run:

```bash
kubectl create -f nginx.yaml # Create the pod
kubectl exec nginx -- ip route # Run IP route command in the pod scheduled on node01
```

## Service Networking

In a cluster, pods communicate with each other through services, using their DNS names or IP addresses, instead of using the pod’s IP which can change if it restarts. While a pod runs on a single node, a service spans the entire cluster and can be reached from any of the nodes. In the above image, the purple service is of type `NodePort`, which means it can be accessed at a given port from any node in the cluster.

Services are allocated IPs from a range configured in the **KubeAPI** server using the `service-cluster-ip-range` parameter. The service networking CIDR should not overlap with the pod networking CIDR.

Services in K8s are not resources like pods. When a service object is created, the **Kube-proxy** running on each node configures the iptable rules on all of the nodes to forward the requests coming to the `IP:port` of the service to the `IP:port` of the backend pod configured in the service.

You will rarely configure pods to communicate directly with each other, if you want a pod to access services on another pod you would always use a service.

* **ClusterIP:** A service that is only accessible within the cluster itself.
* **NodePort:** It's same as clusterIP but it also exposes the app on a port on all nodes in cluster.

{% hint style="warning" %}
Services exist on all nodes in a cluster, they aren't like pods

Services has no interfaces or processes, it's only a virtual object
{% endhint %}

When we create a service, it is given a pre-defined IP address, Kube-proxy daemonset gets this IP address and port then create forwarding rules on each node saying any traffic comes to IP:Port of service should be forwarded to the pod IP address:port.

Kube-proxy have different ways of creating these rules, but if not specified by `kube-proxy --proxy-mode` it defaults to `iptables`

Services IP address range are specified in the kube-apiserver (`--service-cluster-ip-range`)

#### To view rules created by kube-proxy:

#### Also, we can view kube-proxy logs:

```bash
cat /var/log/kube-proxy.log
```

### Lab:

#### To get pods IP address range, check the CNI used and run:

```bash
kubectl -n kube-system logs <cni-pod-name> | grep ipalloc
```

#### To get services IP address range:

```bash
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep range
```

## CoreDNS in Kubernetes

#### Let's say we have 2 pods and we want them to resolve each other:

We need to create entries in the `/etc/hosts` file

{% hint style="warning" %}
This is not efficient when we have thousands of pods and many are being created and deleted
{% endhint %}

So that's why we need to move these entries in a central DNS server which then creates an entry in the `/etc/resolv.conf` file which points to the IP of the DNS server

{% hint style="info" %}
Kubernetes implements DNS the same way, after v1.12 the recommended DNS server is CoreDNS, previously it was kube-dns.
{% endhint %}

CoreDNS is deployed as pods in kube-system namespace, this pods run the CoreDNS executable which requires configuration file located at `/etc/coredns/Corefile`

{% hint style="info" %}
Every record in the coreDNS server falls under cluster.local domain
{% endhint %}

Also, the `Corefile` is passed as a configmap object, so you can edit configmap object to edit the `Corefile`

{% hint style="info" %}
CoreDNS has a service named `kube-dns` so the pods can reach it, it is pre-configured when CoreDNS is deployed and running by kubernetes (kubelet)
{% endhint %}

### Lab:

Before editing any pod, make sure if its part of a deployment, if it is, then edit the deployment not the pod itself.

{% hint style="info" %}
`web-service.payroll.svc.cluster`won't work, `web-service.payroll.svc`, `web-service.payroll`and `web-service.payroll.svc.cluster.local` will work!

You should either use cluster.local or not use them together at all
{% endhint %}

The payroll in this case is the namespace, if you see `web-service.default`, this means its in the default namespace and `cluster.local` is the default domain name used for DNS resolution within the kubernetes cluster.

## Ingress

Ingress is a single entry point into the cluster. It’s basically a layer-7 load balancer that is managed within the K8s cluster. It provides features like **SSL termination**, and **request based routing** to different services.

Ingress uses an existing reverse proxy solution like Nginx or Traefik to run an **Ingress Controller**.

Then a set of ingress rules are configured using definition files. These are called as **Ingress Resources**.

**A K8s cluster does not have an ingress controller by default.** If you just configure ingress resources, it won’t work.

{% hint style="warning" %}
Ingress Controllers are not just regular reverse-proxy solutions. They have additional intelligence built into them to monitor the K8s cluster for new ingress resources and configure themselves accordingly. The ingress controller needs a service account to do this.
{% endhint %}

The ingress controller requires a NodePort Service to be exposed at a node port on the cluster.

Alternatively, the ingress controller requires a LoadBalancer Service to be exposed as a public IP. DNS server can then be configured to point to the IP of the cloud-native NLB.

### Deploying Ingress Controller

* An ingress controller is deployed as just another deployment in K8s.
* The example below uses the build image of ingress controller using Nginx as reverse proxy.
* The program to run the ingress controller is present at `/nginx-ingress-controller` which is passed as `args`.
* Nginx requires some configuration to run as expected. Instead of configuring these in the deployment definition file. These should be decoupled into a ConfigMap `nginx-configuration`. This config file is passed in `args` as well.
* Nginx ingress controller requires two environment variables `POD_NAME` and `POD_NAMESPACE` to be passed. This can be fetched from the metadata.
* Finally, expose the ports on the ingress controller pod to allow the service to reach the pod.

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
          args:
            - /nginx-ingress-controller
            - --configmap=$(POD_NAMESPACE)/nginx-configuration
          env:
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - name: http
              containerPort: 80
            - name: https
              containerPort: 443
```

{% hint style="warning" %}
A NodePort service can then be configured to make the ingress controller accessible at a node port in the cluster.
{% endhint %}

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
      name: http
    - port: 443
      targetPort: 443
      protocol: TCP
      name: https
  selector:
    name: nginx-ingress

```

### Ingress Resource

Ingress resources are set of rules and configuration applied on the ingress controller. This includes path based routing, subdomain based routing, etc.

The backend in the ingress definition file defines the service name and the port at which the application service is running.

{% hint style="success" %}
For every hostname or domain name, we need a separate rule. For each rule, we can route traffic based on the path.
{% endhint %}

#### Ingress resource to route all traffic to a backend service:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear
spec:
  defaultBackend:
    service:
      name: wear-service
      port:
        number: 80

```

#### Path Based Routing:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear
spec:
  rules:
    - http:
        paths:
          - path: /wear
            pathType: Prefix
            backend:
              service:
                name: wear-service
                port:
                  number: 80
          - path: /watch
            pathType: Prefix
            backend:
              service:
                name: watch-service
                port:
                  number: 80

```

{% hint style="warning" %}
If none of the rules or paths match, then the user will be redirected to the default backend service if it is configured in the ingress resource.

So, you should deploy a service by that name if you want to display a nice 404 not found message.
{% endhint %}

#### Hostname Based Routing:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear
spec:
  rules:
    - host: wear.my-online-store.com
      http:
        paths:
          - backend:
              service:
                name: wear-service
                port: 
                  number: 80
    - host: watch.my-online-store.com
      http:
        paths:
          - backend:
              service:
                name: watch-service
                port: 
                  number: 80

```

#### Rewrite target

The `rewrite-target` annotation in an Ingress resource is used to modify the path of the request before it is forwarded to the backend service. It essentially rewrites the URL path.

Consider you have a service, `wear-service`, that is set up to respond to requests at the root path `/`. However, for users or external access, you want this service to be accessible at a path like `/pay`.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: wear.my-online-store.com
      http:
        paths:
          - backend:
              service:
                name: wear-service
                port:
                  number: 80
```

1. A user accesses the URL `wear.my-online-store.com/pay`.
2. The Ingress matches this request to the rule for `/pay`.
3. Due to the `nginx.ingress.kubernetes.io/rewrite-target: /` annotation, the path `/pay` is rewritten to `/`. This means that even though the user accesses the `/pay` path, the request that reaches `wear-service` is as if the user accessed `/`.
4. `wear-service` processes the request as if it came directly to the root path.

### Lab 1:

I wanted to do an ingress controller, but turns out the ingress controller is already deployed as a deployment and I just have to create an ingress resource with the correct namespace, service name and port.

{% hint style="warning" %}
namespace isn't set by default in ingress resource docs, so add it under metadata
{% endhint %}

#### Faster way of creating ingress resource:

```bash
kubectl create ingress ingress-pay -n critical-space --rule="/pay=pay-service:8282"
# Add as many rules with --rule=
```

### Lab 2:

Went well. But I must pay attention to yaml case sensitivity.

## Slides
