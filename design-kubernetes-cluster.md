# Design Kubernetes Cluster

## Basics

* K8s is not supported on Windows. You need to use a virtualization software to run a linux OS on it to support K8s.
* **Minikube** uses a virtualization software to run VMs that host k8s components. This allows us to deploy single node clusters easily.
* **KubeAdm** deploys single or multi node cluster on provisioned VMs for development purposes.

## Deploying Production-Grade Clusters

Production grade clusters can be either setup from scratch (turnkey solution) or they can be managed by a cloud provider (hosted solution).

### **Turnkey Solution**

* You provision VMs
* You configure VMs
* You use scripts to deploy cluster
* You maintain VMs yourself

Example: OpenShift, Vagrant, etc.

### **Hosted Solution**

* Kubernetes-as-a-Service
* Provider provisions and configures VMs
* Provider installs Kubernetes
* Provider maintains VMs

Example: EKS, GKE, OpenShift Online, AKS

## High Availability

**To avoid single point of failure, production grade clusters should have at least 3 master nodes.** Consider a cluster with 2 master nodes.

### API Server

The **API Server** is just a REST API server, it can be kept running on both the masters in an **active-active mode**. To distribute the incoming requests to both the KubeAPI servers, use a load balancer like `nginx` in front and point the `kubectl` utility to the load balancer (by configuring [KubeConfig](https://www.notion.so/KubeConfig-c99bacd10778413fbcb4f580dd0b9dbe?pvs=21)).

### Controller Manager and Scheduler

The **Controller Manager** and the **Scheduler** look after the state of the cluster and make necessary changes. Therefore to avoid duplicate processing, they run in an **active-passive mode**.

The instance that will be the active one is decided based on a leader-election approach. The two instances compete to lease the endpoint. The instance that leases it first gets to be the master for the lease duration. The active instance needs to renew the lease before the deadline is reached. Also, the passive instance retries to get a lease every leader-elect-retry-period. This way if the current active instance crashes, the standby instance can become active.

{% hint style="success" %}
The kube-controller-manager and the kube-scheduler have leader election turned on by default.
{% endhint %}

### ETCD

The ETCD Server can be present in the master node (stacked topology) or externally on other servers (external ETCD topology). Stacked topology is easier to setup and manage but if both the master nodes are down, then all the control plane components along with the state of the cluster (ETCD servers) will be lost and thus redundancy will be compromised.

{% hint style="danger" %}
Regardless of the topology, both the API server instances should be able to reach both the ETCD servers as it is a distributed database.
{% endhint %}

ETCD is distributed datastore, which means it replicates the data across all its instances. It ensures strongly consistent writes by electing a leader ETCD instance (master) which processes the writes. If the writes go to any of the slave instances, they are forwarded to the master instance. Whenever writes are processed, the master ensures that the data is updated on all of the slaves. Reads can take place on any instance.

{% hint style="warning" %}
When setting up the ETCD cluster for HA, use the initial-cluster option to configure the ETCD peers in the cluster.
{% endhint %}

ETCD instances elect the leader among them using **RAFT protocol**. If the leader doesn’t regularly notify the other instances of its role regularly, it is assumed to have died and a new leader is elected.

A write operation is considered complete only if it can be written to a majority (**quorum**) of the ETCD instances (to ensure that a write goes through if a minority of ETCD instances goes down). If a dead instance comes back up, the write is propagated to it as well.

`Quorum = floor(N/2 + 1)`, where N is the total number of ETCD instances, is the minimum number of ETCD instances that should be up and running for the cluster to perform a successful write.

`N = 2 => Quorum = 2`, which means quorum cannot be met even if a single instance goes down. So, having 2 instances is the same as having a single instance (0 fault tolerance). So, **we should have at least 3 instances in the ETCD cluster as it offers fault tolerance of 1 instance.**

Considering 1 ETCD instance per master node, **it is recommended to have odd number of master nodes in the cluster.** This is because, during a network segmentation, there is higher changes of quorum being met for one of the network segments.

Since 1 and 2 nodes don’t provide any fault tolerance, the minimum number of nodes for fault tolerance is 3. Also, the even number of nodes can leave the cluster without quorum in certain network partitions. So, **the most commonly used node count is 3 (good for most use cases) and 5 (higher fault tolerance).**

## Slides
