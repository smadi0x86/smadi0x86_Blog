# Core Concepts

## Cluster Architecture

1. **ETCD Cluster**: A highly available key-value store used by Kubernetes to store all cluster data, ensuring data consistency and cluster state across the cluster.
2. **kube-apiserver**: The central control plane component that exposes the Kubernetes API, acting as the front-end for the Kubernetes control plane.
3. **kube-scheduler**: Determines which nodes can run a pod based on resource availability, then assigns the pod to run on a specific node.
4. **Kube Controller Manager**: Runs controller processes to regulate the state of the system, managing different non-terminating control loops.
5. **kubelet**: An agent running on each worker node, responsible for ensuring that containers are running in a Pod.
6. **Kube-proxy**: Maintains network rules on nodes, allowing network communication to Pods from network sessions inside or outside the cluster.
7. **Container Runtime Engine**: The software responsible for running containers, with Docker and rkt being common choices.

### Docker vs Containerd

#### The Story

At the beginning of the container era, there was only docker and other tools like `rkt` , docker made working with containers super-easy, so Kubernetes was created to orchestrate docker, Kubernetes and docker were tightly coupled.

Kubernetes grew in popularity as a container orchestrator, so Kubernetes users needed it to work with other container runtime other than docker, so Kubernetes introduced a `Container Runtime Interface (CRI)` and this allowed any vendor to work as a container runtime for Kubernetes as long as they adhere to `Open Container Initiative (OCI) Standard`, so this allowed anyone to build a container tool that can be used to work with Kubernetes.

Docker was built way before the `OCI Standard` was introduced, even with that, docker remained the dominant container tool used by many, so Kubernetes had to keep it and introduce a way to make it work.

To still maintain docker, Kubernetes introduced a hacky temporary way called `dockershim`.

While every container runtime worked with Kubernetes through `CRI`, docker wasn't.

Docker isn't just a container runtime, it consists of many other tools such as: docker api, cli, build tools, volume for storage, auth for security and container runtime (runc) which was managed by the daemon containerd.

Containerd is CRI Compatible and can work directly with Kubernetes like others, and can be used as a runtime on its own separate from docker.

So Kubernetes decided to stop maintaining the dockershim in Kubernetes v1.24 as it was unnecessary effort and that also removed support for docker, although docker images can still be used because docker followed the OCI imagespec standard but docker was removed as a supported runtime from Kubernetes.

#### Containerd

Containerd is now a separate project that is a member of CNCF graduated status, and you can now install containerd on its own without having to install docker engine.

#### So if docker isn't installed, how would we run containers in containerd?

containerd comes with a cli tool called `ctr`

ctr isn't user friendly and mainly used debugging and is not meant for running or managing containers on a production environment.

Nerdctl CLI is a better alternative for managing and running containers and it has almost all commands available in docker and also provides newest features of containerd such as: Encrypted Container images, p2p image distribution, lazy pulling, Image signing and verifying, Kubernetes namespaces which isn't available in docker.

Crictl provides a CLI for CRI Compatible container runtimes, it's created and maintained by the Kubernetes community.

Crictl is installed seperately and works across different container runtimes and it's used to inspect and debug container runtimes and not used to create containers (technically possible but not easy) and used for special debugging purposes.

It works along with kubelet, we know that kubelet is responsible to ensure a certain number of pods/containers are available on a node, if you try to create containers with crictl, kubelet will delete them as its not aware of these container and its out of its knowledge.

Crictl is similar to docker syntax.

{% embed url="https://kubernetes.io/docs/reference/tools/map-crictl-dockercli/" %}

Prior to kubernetes v1.24, crictl connected to these endpoints in the following order:

However, by the release of kubernetes v1.24, a significant change was made, the dockershim.sock was replaced with cri-dockerd.sock as a result the updated default endpoints was changed.

Users are encouraged to manually set the endpoints:

#### In summary

### ETCD

#### Objectives

* What is ETCD?
* What is a Key-Value Store?
* How to get started quickly?
* How to operate ETCD?
* What is a distributed system?
* How ETCD Operates
* RAFT Protocol
* Best practices on number of nodes

#### What is ETCD?

ETCD is a distributed key-value store that is simple, Secure, and Fast.

#### Installing ETCD is simple:

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash"><strong>wget https://github.com/etcd-io/etcd/releases/download/v3.5.12/etcd-v3.5.12-linux-amd64.tar.gz
</strong>
<strong>tar xzvf etcd-v3.5.12-linux-amd64.tar.gz
</strong>
<strong>./etcd # Runs at port 2379 
</strong></code></pre>

{% embed url="https://etcd.io/docs/v3.5/install/" %}

#### Operate ETCD

ETCD comes with a command line tool called `etcdctl`

```bash
./etcdctl set key1 value1 # Set a key-value store
./etcd get key1 # Get value of a key
./etcdctl -h # Additional commands
```

#### ETCD Versions

With the release of etcd version 3.4 the default API version is set to 3

To change etcd to work with API version 3, you can either set it in every command or export for entire session, also now you can run `./etcdctl` and it will display version not like v2 where you had to run `./etcdctl --version`:

Also, in v3 the command to set a key-value store changed to:

### ETCD in Kubernetes

ETCD Key-Value store in Kubernetes stores:

Everything you see when you run `kubectl get` command is from the etcd server and every change you make to your cluster such as adding additional nodes, deploying pods, replica sets are updated in the etcd server, only once it is updated/added in etcd server then its considered complete.

Depending on how you setup your cluster, etcd is deployed differently. We'll take a look on setting it up from scratch and using kubeadm.

#### Setup From Scratch

If you setup the cluster from scratch you have to setup etcd from scratch by installing its binaries, and configuring etcd as a service yourself.

{% hint style="info" %}
ETCD Listens on --advertise-client-urls and its on the IP of server and port 2379 which is the default port that etcd listens on. So this is the URL that need to be configured on the KubeAPI server so it can reach the etcd server
{% endhint %}

#### Setup with Kubeadm

Kubdeadm deploys etcd server as a pod in the kube-system namespace.

You can explore etcd database using etcdctl within etcd pod:

To list all keys stored by Kubernetes:

Kubernetes stores data in the specific directory structure, the root directory is a registry and under it you have minions, pods, replica sets etc...

#### ETCD in High Availability Environment

In HA, there are multiple master nodes in the cluster, then there will be multiple etcd instances across the master nodes, so make sure the etcd instances know about each other by setting the right parameter in the `etcd.service` configuration.

#### ETCD Commands (Optional)

ETCDCTL is the CLI tool used to interact with ETCD.

ETCDCTL can interact with ETCD Server using 2 API versions - Version 2 and Version 3.

By default its set to use Version 2. Each version has different sets of commands.\\

For example ETCDCTL version 2 supports the following commands:

```bash
etcdctl backupetcdctl cluster-healthetcdctl mketcdctl mkdiretcdctl set
```

Whereas the commands are different in version 3:

```bash
etcdctl snapshot save etcdctl endpoint healthetcdctl getetcdctl put
```

To set the right version of API set the environment variable ETCDCTL\_API command

`export ETCDCTL_API=3`

When API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don't work. When API version is set to version 3, version 2 commands listed above don't work.

Apart from that, you must also specify path to certificate files so that ETCDCTL can authenticate to the ETCD API Server.

The certificate files are available in the etcd-master at the following path. We discuss more about certificates in the security section of this course.

So don't worry if this looks complex:

{% code overflow="wrap" %}
```bash
--cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key
```
{% endcode %}

So for the commands I showed in the previous video to work you must specify the ETCDCTL API version and path to certificate files. Below is the final form:

{% code overflow="wrap" %}
```bash
kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
```
{% endcode %}

### KubeAPI Server

When we run `kubectl get nodes` we are reaching to the KubeAPI server which:

* Authenticates request
* Validates request
* Retrieve data from ETCD server

Lets give an example of creating a pod using a post request `curl -X POST /api/v1/namespaces/default/pods ...`

* Authenticate the request
* Validate the request
* KubeAPI server creates a pod object without assigning it to a node
* Updates information in ETCD server
* Updates user that pod has been created
* Scheduler monitors KubeAPI server continuously and realize that there is a new pod with no node assigned
* Scheduler identifies new node to put the pod on and communicate it back to KubeAPI server
* KubeAPI server updates information in ETCD server
* KubeAPI server pass this information to the kubelet in appropriate worker node
* Kubelet creates pod on the node and constructs the container runtime engine to deploy the app image
* Kubelet updates status back to KubeAPI server
* KubeAPI server updates data in ETCD server

{% hint style="info" %}
KubeAPI server is the only component which interacts directly with the ETCD server
{% endhint %}

### Kube Controller Manager

A controller is a component which continuously monitors the state of various components in the system and works to get the whole system to the desired state.

Node-Controller is responsible for monitoring state of nodes and does it through KubeAPI server, Node-Controller checks the state of the nodes every 5 seconds, if it stops receiving heartbeats from nodes its marked unreachable but it waits for 40 seconds before, after its marked it gives it 5 minutes to come back up, if it doesn't it removes pods and move them to another node.

Replication-Controller is responsible of monitoring status of replica sets and make sure desired number of pods is achieved at all times.

These were only 2 parts of Kube-Controller, there are a lot of components under Kube-Controller Manager but they offer same goal and its the brain behind everything in Kubernetes.

### Kube Scheduler

Scheduler is only responsible on deciding which pod goes on which node, it doesn't actually place pod on the node which is the responsibility of the kubelet which creates the pod on the node.

#### Why do we need a scheduler?

To make sure pods are placed on the right node.

#### For example we have a pod which needs 10 CPUs power and we have 4 nodes:

**Node 1:** 4 CPUs

**Node 2:** 4 CPUs

**Node 3:** 12 CPUs

**Node 4:** 16 CPUs

#### Scheduler goes into two phases:

1\) Filter out nodes which doesn't meet requirements

2\) Scheduler then rank nodes, which calculates how many CPU left after the pod is placed in each node

So the 4th node was picked by the scheduler and the pod will be placed there!

### Kubelet

The kubelet in the kubernetes worked node registers the node with a Kubernetes cluster, when it receives instructions to load a pod it requests the container runtime engine to pull the required image and run the instance.

Kubelet then monitors the state of the pod and containers in it and reports to the KubeAPI server on timely basis.

{% hint style="danger" %}
IMPORTANT: Kubdeadm does not deploy kubelets by default as a pod and we must manually install kubelets on the worked nodes
{% endhint %}

### Kube Proxy

Kube proxy is a process which runs on each node, its job to look for new services and create appropriate rules on each node to forward traffic to those services to the backend pods such as using iptables.

### Pods Recap

A pod is a single instance of an application and is the smallest object you can create in Kubernetes.

Pods usually have a 1 to 1 relation to containers, so to scale up we create additional pods and if we want to scale down we delete existing pods, we must not create more than one container in the same pod.

{% hint style="warning" %}
A single pod can have multiple containers but they should be related such as a helper container for supporting tasks for our application such as processing user data, the 2 container can refer to each other as localhost (same network) and share same storage space
{% endhint %}

The `kubectl run nginx --image nginx` command automatically creates a pod with nginx image for a web server. In the current state of this pod, it can't be accessed by the public internet.

### Pods with YAML

All Kubernetes yaml contains 4 top-level fields which are required:

```yaml
# myapp.yml

apiVersion: v1 # Version of KubeAPI
kind: Pod  # Type of object
metadata: # Data about the object (dictionary)
    name: myapp # Pod name
    labels: # Any Key-Value pairs (same as tags)
        app: myapp
        type: backend
        creator: smadi
        
spec: # Specifications, where we provide additional information regarding the pod
    containers: # List/Array
        - name: nginx-container
          image: nginx

```

```bash
kubectl apply -f myapp.yml
kubectl get pods
kubectl describe pod <pod_name>
```

### Lab 1: Pods

Issues encountered was using editors, nano was slow and I got yaml issues, vim was a little better but not that good also yaml issues (forgot to add v letter in apiVersion), also I didn't utilize the `kubectl run nginx --image` nginx command, instead I created the yaml myself.

Also I tried to delete the pod using `kubectl delete -f nginx.yml` which kubelet kept re-creating it, instead I had to do `kubectl delete pod <pod_name>`

I should have utilize `kubectl describe` pod more frequently for information about images, containers etc...

Huge mistake I did is trying to hardcode the yaml pods, instead I can generate one with `kubectl run redis --image=redis --dry-run=client -o yaml > redis.yaml`

### ReplicaSets Recap

To create a ReplicaSet yaml:

<pre class="language-yaml" data-overflow="wrap"><code class="lang-yaml"><strong># replicaset-def.yml
</strong>
<strong>apiVersion: apps/v1 # v1 to match ReplicationController kind
</strong>kind: ReplicaSet # Or ReplicationController (Deprecated/Old)
metadata:
    name: myapp-replicaset
    type: backend

spec:
    template:
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                type: backend
            spec:
                containers:
                    - name: nginx-container
                      image: nginx
     replicas: 3
     selector: # Identify what pods fall under it, it can manage other pods which were created before the replicaset (not required) and not used by old ReplicationController
         matchLabels:
             type: backend
     
</code></pre>

To update the scale (replicas) from 3 to 6 for instance:

* `kubectl scale --replicas=6 -f replicaset-def.yml` (won't change it in yaml)
* vim replicaset-def.yml and change replicas to 6 then `kubectl replace -f replicaset-def.yml`

Then you can `kubectl get replicaset` to get all replicasets pods.

### Lab 2: ReplicaSets

Had to check how to delete all pods, it can be done using `kubectl delete pods --all`

Deleted selector and it was needed in yaml definition with a matchLabels, that caused some delays in the `kubectl create -f replicaset.yml`

Usage of `kubectl explain replicaset` to check what the replicaset yaml expects

### Deployments

To create a deployment yaml:

<pre class="language-yaml"><code class="lang-yaml"><strong># myapp-deployment.yml
</strong>
<strong>apiVersion: apps/v1
</strong>kind: Deployment 
metadata:
    name: myapp-deployment
    type: backend
spec:
    template:
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                type: frontend
        spec:
            containers:
            - name: nginx-container
              image: nginx
    replicas: 3
    selector:
        matchLabels:
            type: backend
</code></pre>

`kubectl create -f myapp-deployment.yml`

`kubectl get deployments`

{% hint style="info" %}
Deployments also creates a replicaset by default
{% endhint %}

`kubectl get rs`

### Certification Tip

As you might have seen already, it is a bit difficult to create and edit YAML files. Especially in the CLI. During the exam, you might find it difficult to copy and paste YAML files from browser to terminal. Using the `kubectl run` command can help in generating a YAML template. And sometimes, you can even get away with just the `kubectl run` command without having to create a YAML file at all. For example, if you were asked to create a pod or deployment with specific name and image you can simply run the `kubectl run` command.

Use the below set of commands and try the previous practice tests again, but this time try to use the below commands instead of YAML files. Try to use these as much as you can going forward in all exercises

Reference (Bookmark this page for exam. It will be very handy):

[https://kubernetes.io/docs/reference/kubectl/conventions/](https://kubernetes.io/docs/reference/kubectl/conventions/)

**Create an NGINX Pod**

`kubectl run nginx --image=nginx`

**Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)**

`kubectl run nginx --image=nginx --dry-run=client -o yaml`

**Create a deployment**

`kubectl create deployment --image=nginx nginx`

**Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)**

`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml`

**Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run) and save it to a file.**

`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml`

**Make necessary changes to the file (for example, adding more replicas) and then create the deployment.**

`kubectl create -f nginx-deployment.yaml`

**OR**

**In k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas.**

`kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`

### Lab 3: Deployments

Make sure to check the case-sensitivity in yaml definitions, everything else went well.

Also to recap here is the command I used for creating a new deployment `kubectl create deployment frontend-app --image=httpd-alpine --replicas=3 --dry-run=client -o yaml > new-dep.yml` then `kubectl create -f new-dep.yml`

### <mark style="color:yellow;">Services</mark>

Services enable connectivity between group of pods, enables frontend app to be available to end-users, frontend to be reachable by backend.

#### Use case of services: we deployed a web app on a pod but how will it be accessible by external users?

We need to access the pod IP directly from our laptop to be able to access the web app, here is where the service plays a crucial role, service is an object and its use case is to listen to NodePort (node port service) on the node and forward it to the pod.

#### Service Types:

* **NodePort:** Where service make internal pod accessible on a port on the node
* **ClusterIP:** Service creates a virtual IP inside the cluster to enable communication between different services such as frontend to backend servers
* **LoadBalancer:** Provisions a load balancer for our application in supported cloud provider

#### 1) NodePort

NodePorts have a port range between 30000 - 32767.

#### Defining a service yaml definition with a NodePort type:

```yaml
# service-def.yml

apiVersion: v1
kind: Service
metadata:
    name: myapp-service
    
spec:
    type: NodePort # Service Type
    ports:
    - targetPort: 80 # If not defined, will be same as port
      port: 80 # Port on service object (mandatory)
      nodePort: 30008 # if not defined manually, will allocate a random number
    selector: # Pull labels defined in pod definition and place them here
        app: myapp
        type: frontend
```

`kubectl create -f service-def.yml`

Now we can use the NodePort "30008" to access the web server

If we have more than one pod in same node with same labels, the NodePort uses a random algorithm to distribute load to them.

If pods are on multiple nodes, when we create a service, Kubernetes creates a service which spans across all nodes in the cluster and uses same NodePort on all node IPs which you can access your app using any IP in the node cluster with the same NodePort defined.

{% hint style="info" %}
No additional steps needed for these 2 scenarios, just a normal service definition and Kubernetes takes care of the rest
{% endhint %}

#### ClusterIP

A fullstack app typically have different pods running different components such as:

* Backend pods
* Frontend pods
* Database pods

These pods need to communicate with each other and each pod have different dynamic IPs which may change.

A Kubernetes service can help us group all backend/frontend/database pods to a single interface.

Each service gets an IP and a name assigned to it, this is the name which should be used by other pods to access the service, this service type is known is clusterIP, the default service type is clusterIP.

To create a clusterIP service definition yaml:

```yaml
# service-def.yml

apiVersion: v1
kind: Service
metadata:
    name: backend
    
spec:
    type: ClusterIP
    ports:
    - targetPort: 80
      port: 80
    selector:
        app: myapp
        type: backend
```

#### LoadBalancer

Cloud providers offer native support for Kubernetes LoadBalancers.

The only thing we have to do is change the service type to `LoadBalancer`

### Lab 4: Services

Everything went well, but I didn't pay attention to requirements regarding NodePorts, port, targerPort and service type values given.

### Namespaces

Kubernetes creates pods and services for its internal use, ones required for networking, scheduling etc..., to avoid users accidentally deleting these core components, Kubernetes creates them in a `kube-system` namespace.

#### Common namespaces:

* **Default:** The namespace which is created by default for common user-related pods
* **kube-system:** Kubernetes components pods created for internal Kubernetes functionality
* **kube-public:** Resources that need to be available to all users are created

When you grow and use Kubernetes cluster for enterprise, you must use namespace isolation (separation of concerns) for isolation, for instance different namespaces for different environments (dev, staging, prod).

Every namespace can have its own set of policies that define what could do what.

You can also assign quota of resources to each namespace (resource limits), so each namespace can guarantee a certain amount of resources and doesn't use more.

In default namespace, you can refer to resources within the namespaces simply by their names.

If required, the webapp pod can reach a service in another namespace. It can be done because when a service is created a DNS-entry is added.

When creating a pod like `kubectl create -f pod-deployment.yml` its by default placed in the default namespace.

To create a pod in another namespace (e.g: dev), `kubectl create -f pod-deployment.yml -n dev`

#### Also, you can add a pod to a namespace using the yaml definition to ensure it's always created in same namespace:

#### To create a new namespace, you can do it using yaml definition:

```yaml
# namespace-dev.yml

apiVersion: v1
kind: Namespace
metadata:
    name: dev
```

```bash
kubectl create -f namespace-dev.yml
```

#### Also, you can create a new namespace using:

```bash
kubectl create namespace dev
```

#### To switch to a different namespace permanently to avoid specifying the `-n dev` with every command, we could:

`kubectl config set-context $(kubectl config current-context) --namespace=dev`

#### To specify resource quota limits to namespace, we could:

```yaml
# compute-quota.yml

apiVersion: v1
kind: ResourceQuota
metadata:
    name: compute-quota
    namespace: dev

spec:
    hard:
        pods: "10"
        requests.cpu: "4"
        requests.memory: 5Gi
        limits.cpu: "10"
        limits.memory: 10Gi
```

```bash
kubectl create -f compute-quota.yml
```

### Lab 5: Namespaces

Everything went well, had issues creating a pod, used `kubectl create redis --image redis -n marketing` which is wrong, I need to specify what I wanna create which in this case was pod.

Correct command: `kubectl create pod redis --image redis -n marketing` or `kubectl run redis --image redis -n marketing`

### Imperative vs Declarative

#### **Imperative Approach in Kubernetes:**

* **What it is**: Giving specific commands to create or modify resources (e.g., `kubectl create -f pod.yaml`).
* **Pros**: Direct control, good for specific tasks and scripts.
* **Cons**: Requires detailed knowledge of each operation, less scalable.
* **Use Case**: Best for development, debugging, and one-off tasks.

#### **Declarative Approach in Kubernetes:**

* **What it is**: Defining the desired state in YAML files and letting Kubernetes maintain this state (e.g., `kubectl apply -f pod.yaml`).
* **Pros**: Easier to manage at scale, aligns with version control, system handles complexities.
* **Cons**: Less intuitive for imperative command users, requires understanding of YAML configurations.
* **Use Case**: Ideal for production and long-term configuration management.

### Certification Tips - Imperative Commands with Kubectl

While you would be working mostly the declarative way - using definition files, imperative commands can help in getting one time tasks done quickly, as well as generate a definition template easily. This would help save considerable amount of time during your exams.

Before we begin, familiarize with the two options that can come in handy while working with the below commands:

`--dry-run`: By default as soon as the command is run, the resource will be created. If you simply want to test your command , use the `--dry-run=client` option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

`-o yaml`: This will output the resource definition in YAML format on screen.

Use the above two in combination to generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch.

#### **POD**

**Create an NGINX Pod**

`kubectl run nginx --image=nginx`

**Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)**

`kubectl run nginx --image=nginx --dry-run=client -o yaml`

**Deployment**

**Create a deployment**

`kubectl create deployment --image=nginx nginx`

**Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)**

`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml`

**Generate Deployment with 4 Replicas**

`kubectl create deployment nginx --image=nginx --replicas=4`

You can also scale a deployment using the `kubectl scale` command.

`kubectl scale deployment nginx --replicas=4`

**Another way to do this is to save the YAML definition to a file and modify**

`kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml`

You can then update the YAML file with the replicas or any other field before creating the deployment.

#### **Service**

**Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379**

`kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`

(This will automatically use the pod's labels as selectors)

Or

`kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml` (This will not use the pods labels as selectors, instead it will assume selectors as **app=redis.** [You cannot pass in selectors as an option.](https://github.com/kubernetes/kubernetes/issues/46191) So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)

**Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:**

`kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml`

(This will automatically use the pod's labels as selectors, [but you cannot specify the node port](https://github.com/kubernetes/kubernetes/issues/25478). You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

`kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`

(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

#### **Reference:**

[https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

[https://kubernetes.io/docs/reference/kubectl/conventions/](https://kubernetes.io/docs/reference/kubectl/conventions/)

### Lab 6: Imperative Commands

Main issue was creating a service and exposing it using imperative commands, also I was creating a different service suing its own imperative command while I could have done `kubectl run redis --image redis --port=8080 --expose`

Also, I was using `kubectl create pod`which is not correct at all, if creating pods I must do `kubectl run`

### Kubectl Apply

## Slides
