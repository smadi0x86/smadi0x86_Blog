# Scheduling

## Manual Scheduling

Every pod definition yaml have a field called nodeName which is by default not set, we don't typically specify it and Kubernetes specify it using its scheduling algorithm.

If there are no nodes available, the pod will be in a pending state.

If we don't have a scheduler in our Kubernetes cluster, we must specify the nodeName ourselves in the pod definition yaml.

#### We can only specify nodeName on creation time, but what if we have an existing pod and we want to assign a node for it?

We must create a binding object and send a post request to the pod binding API, thus mimicking what an actual scheduler does.

### Lab:

I didn't know where the nodeName must be (forgot its definition), it mus be in under spec block.

Also instead of deleting, I got ran `kubectl replace --force -f nginx.yaml.`

I could have checked the scheduler pod in kube-system for debugging purposes.

## Labels & Selectors

Labels and selectors are used to group resources and filter them based on a criteria using key-value pairs (similar to tags).

When you add labels to a Kubernetes definition manifest, you can use `kubectl get pods --selector app=app1` to get pods with the label app which has a value app1.

Selectors are also used in ReplicaSets manifests to group and discover the pods.

### Annotations

Labels and selectors are used to group and select objects, annotations are used to record other details for informatory purposes that may be used for integrations for instance.

### Lab:

All went well, there was an error from the same lab but I made sure I understood everything.

## Taints and Tolerations

Taints and Tolerations are used to set restrictions on what pods can be scheduled on what nodes.

If we want to schedule certain pods in a single node, we first put a taint on the node we want to restrict any pods from being placed there, lets say the taint value is "blue". So now non of the pods can be placed on node01, then we'll specify what pods are tolerant to the tainted node. Pod D is tolerant to node01 taint.

```
kubectl taint nodes node01 app=blue:NoSchedule
```

#### Also, we can add these tolerations in the yaml definition of the pod:

{% hint style="info" %}
Taints and Tolerations doesn't tell pods to go to a particular node, instead it tells the node to only accept pods with certain tolerations.
{% endhint %}

The master node where it runs all management software and the capabilities of running a pod, the scheduler doesn't create pods on the master node due to a taint set by default when you create your Kubernetes cluster.

### Lab:

I had a lot of mistakes specially the tainting pods and untainting nodes, the pods doesn't have an imperative command for tainting so I must `--dry-run=client -o yaml > file.yaml` and add the tolerations under spec.

Also, for nodes untaining, I must describe the node I want to untaint, then go and copy the taints and run `kubectl taint nodes <nodename> <copied taint>-`

## Node Selectors

The key-value of size and large pairs are labels created on nodes.

#### To create a label on a node:

```
kubectl label nodes <node-name> <label-key>=<label-value>
```

## Node Affinity

Primary purpose of node affinity is to make sure pods are placed on particular nodes.

The node affinity provides advanced expressions to limit pod placement on specific nodes.

#### What if node affinity could not match a node with given expressions (no nodes with label called size?), will the pod continue to stay on the node?

This is answered by the node affinity types

### Lab:

Many issues were encountered at first, I tried to add affinity block to the deployment yaml (I created), but this is not efficient as when I apply the deployment, the kubernetes will store a more precise and efficient yaml in its memory, so I did `kubectl edit deployment <dep_name>`, then added the affinity under the spec which is under the template, also I had hard time in indentation.

Also, the operators were used incorrecly by me at first, the Exists and DoesNotExist operator doesn't require a value because it checks the key if its in the deployment or not, if it exists then it will be in the corresponding node which has the key label defined, if it DoesNotExist, it will be placed in any node which doesn't have the key label.

## Resource Requirements and Limits

You can specify resource requests on pods definitions to specify how many CPU and Memory a pod can use.

So when the scheduler gets a request to look for a node to place this pod in, it looks for a node that has this amount of resources available and after the pod get placed on the node, it gets guaranteed amount that it specified using the resource requests.

{% hint style="info" %}
By default, a pod has no limits to resources it can consume on a node.
{% endhint %}

#### You can set a limit using resource limits to the pod definition:

If a pod tries to get more CPU than that limit provided, it will be throttled, although if a pod tries to exceed the memory limit it can, but the pod will be terminated with a OOM (Out of memory) error.

If resource requests aren't specified and limits are specified, Kubernetes will set requests the same value as the limits specified.

#### How do we ensure that every pod created has a some defaults set regarding resources?

We can use LimitRange definition yaml

{% hint style="info" %}
Note that these limits are enforced when pods are created, if you create or change a limit range it doesn't effect existing pods.
{% endhint %}

Also, we can create quotas at the namespace level to set hard limits for all pods on a given namespace.

### Lab:

Went really well, I used the Quick Note help below to reapply the pod.

## Quick Note

### **Edit a POD**

Remember, you CANNOT edit specifications of an existing POD other than the below.

* spec.containers\[\*].image
* spec.initContainers\[\*].image
* spec.activeDeadlineSeconds
* spec.tolerations

For example you cannot edit the environment variables, service accounts, resource limits (all of which we will discuss later) of a running pod. But if you really want to, you have 2 options:

1\. Run the `kubectl edit pod <pod name>` command. This will open the pod specification in an editor (vi editor). Then edit the required properties. When you try to save it, you will be denied. This is because you are attempting to edit a field on the pod that is not editable.

<figure><img src="https://img-c.udemycdn.com/redactor/raw/2019-05-30_14-46-21-89ea56fea6b993ee0ccff1625b13341e.PNG" alt=""><figcaption></figcaption></figure>

<figure><img src="https://img-c.udemycdn.com/redactor/raw/2019-05-30_14-47-14-07b2638d1a72cb2d5b000c00971f6436.PNG" alt=""><figcaption></figcaption></figure>

A copy of the file with your changes is saved in a temporary location as shown above.

You can then delete the existing pod by running the command:

`kubectl delete pod webapp`

Then create a new pod with your changes using the temporary file

`kubectl create -f /tmp/kubectl-edit-ccvrq.yaml`

2\. The second option is to extract the pod definition in YAML format to a file using the command

`kubectl get pod webapp -o yaml > my-new-pod.yaml`

Then make the changes to the exported file using an editor (vi editor). Save the changes

`vi my-new-pod.yaml`

Then delete the existing pod

`kubectl delete pod webapp`

Then create a new pod with the edited file

`kubectl create -f my-new-pod.yaml`

**Edit Deployments**

With Deployments you can easily edit any field/property of the POD template. Since the pod template is a child of the deployment specification, with every change the deployment will automatically delete and create a new pod with the new changes. So if you are asked to edit a property of a POD part of a deployment you may do that simply by running the command

`kubectl edit deployment my-deployment`

## DaemonSets

The daemonset ensures that one copy of the pod is always present in all nodes in the cluster.

A use case could be a monitoring solution or logs viewer, as it can be deployed in all nodes on the cluster, so you don't have to worry about adding and removing monitoring and logs viewer agents in your nodes as daemonsets will take care of that.

{% hint style="info" %}
Kube-proxy is deployed as a daemonset for networking to be available on all nodes. This is a great example of daemonsets.
{% endhint %}

#### To create a daemonset, its very similar to replicasets:

You can view daemonsets with `kubectl get daemonsets` command.

#### How does it work?

Remember the nodeName that allowed scheduler to place pods on nodes directly? This is how daemonsets used to be placed on all nodes until Kubernetes v1.12.

{% hint style="info" %}
From v1.12 onwards daemonsets uses NodeAffinity rules and default scheduler to schedule pods on nodes.
{% endhint %}

### Lab:

All went well! but I must make sure I edit the right name, I edited the container name instead of the pod name which is not what was needed.

## Static Pods

What if we didn't have a cluster and only had a kubelet with a container runtime, can we still run pods?

Yes, we can still run pods by putting all yaml pod definition in a given folder and configure the kubelet to point to that folder, so the kubelet will create pods from this folder and check on it every once in a while, also it can fix breaking pods and re-start them. If a pod definition is removed from the folder, it is deleted automatically by the kubelet.

{% hint style="info" %}
Kubelet works at a pod level and can only understand pods, Only pods can be created this way, this doesn't include replicasets, deployments etc...
{% endhint %}

To set a folder so the kubelet can create pods from the kubelet config.

Also, you can provide a path to another config file using the `--config=` and define the StaticPodPath in the file, this is used by clusters made with kubeadm tool.

We can check the containers created using the `docker ps` because we don't have the rest of the components of our cluster that works with `kubectl`.

Although, we created the pod statically, if we are part of a Kubernetes cluster, it is mirrored in the kubeAPI server which is only a read-only object, we can't edit or delete it.

#### Why use static pods?

To deploy control plane components as pods on a node, so we don't have to download binaries and deal with crashing of them and overheads, so if a component in our control plane crashes, the kubelet restarts it.

### Lab:

Mistakes happened as I didn't know the paths of kubelet config.

The kubelet config which has path of manifests folders is at `/etc/var/lib/kubelet/config.yaml`

Usually, by default the manifests folder is found at `/etc/kubernetes/manifests/`

Also, I have to pay attention if I need to ssh to a specific node, the config and paths differ from node to node, I can move to other nodes by `ssh <node-name>`

## Multiple Schedulers

### Deploying Additional Schedulers as a pods

#### We create a scheduler definition yaml file:

* \--kubeconfig: Path to the scheduler authentication info to connect to the KubeAPI server
* \--config: Custom kube-scheduler config file, which has the name of the scheduler
* LeaderElection: Is used when we have multiple copies of same scheduler running on multiple master nodes in a high availability setup, where it chooses a leader to lead the scheduling activities as only one scheduler can be activated at a time.

{% embed url="https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/" %}

#### To configure a pod to use a custom scheduler:

#### To view the scheduler events:

```bash
kubectl get events -o wide -n kube-system # List all events in current namespace
# Or
kubectl logs my-custom-scheduler -n kube-system # Debugging logs
```

### Lab:

All went well.

## Configuring Scheduler Profiles

When a scheduler wants to schedule a pod on a certain node, there are steps that happen before.

### Scheduling Phase

Scheduling queue, where a scheduler look for a priority class name which can be included in a pod definition file and the priority class created alone and set a value of 10 million which is ultra high priority.

### Filtering Phase

Then our pod enters the filtering phase, where nodes that cannot run the pod are filtered out.

### Scoring Phase

Where nodes are scored with different weights, based on the free spoce after the pod is placed on the node, if a node gets 2 score after pod is placed and the other node gets 6, then the node with 6 will win.

### Binding Phase

This is where the pod is binded to the node with the highest score.

All of these phases has plugins which include algorithms to calculate specific stuff related to the phase aim.

For instance in the Filtering phase, there is a plugin called `NodeUnschedulable` which looks for Unschedulable flag set to True. So this plugin makes sure pods aren't placed on nodes where this flag is set to true.

### Extension Points

At each stage of scheduling, there is an extension point where each plugin is plugged to, you can get a custom code to run anywhere by creating a plugin and plugging it in the respective extension point.

With Kubernetes v1.18, a feature to allow more profile entries to the same scheduler, which removes the need to create different scheduler binaries for different profiles.

To configure each profile on our scheduler binary, we can configure the plugins on each profile as per our need.

{% embed url="https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work" %}

{% embed url="https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/" %}

## Slides
