# Kubernetes The Kubeadm Way

## Kubeadm

* Create the nodes to be used in the cluster
* Setup the nodes - forwarding IPv4 and letting iptables see bridged traffic on all the nodes - [Container Runtimes | Kubernetes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#install-and-configure-prerequisites)
* Install a [container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) on all of the nodes (`containerd` recommended)
* Install `kubeadm`, `kubelet` and `kubectl` on all the nodes, refer [Installing kubeadm | Kubernetes](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
* Initialize the control plane node with the pod networking CIDR and the API server endpoint as the control plane’s IP address in its local network so that the worker nodes can reach the API server on the master node’s IP - `kubeadm init --apiserver-advertise-address 10.33.92.10 --pod-network-cidr=10.244.0.0/16`
*   The above command will create a file `admin.conf` in `/etc/kubernetes` directory which can be used to authenticate to `kubectl`. Follow the on-screen instructions to move this file to the `.kube` folder in the user’s home directory.

    We can now run `kubectl` commands from the master node.
* The output of the `kubeadm init` command returns a `kubeadm join` command that needs to be run on all the worker nodes to join them with the master node.
* Deploy the cluster networking solution (eg. WeaveNet) as a DaemonSet on all the nodes by running a single `k apply` command on the master node, refer [Integrating Kubernetes via the Addon (weave.works)](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/). Configure the networking solution to use the same CIDR as the pod network configured in `kubeadm init` command.

{% hint style="danger" %}
When the K8s cluster is deployed using `kubeadm`, all the control plane components (except the `kubelet`) are deployed as static pods in the `kube-system` namespace.

The manifest files for these components are located at `/etc/kubernetes/manifests/`.

Simply editing these manifest files leads to the static pods restarting with the updated config.
{% endhint %}

## Lab

The only thing that was not clear is adding the iface in the Flannel definition yaml, it wasn't mentioned in the lab writeup.

Also, when trying to get yaml from github, make sure to go to the file itself and copy the `raw url`

{% code overflow="wrap" %}
```bash
wget https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
```
{% endcode %}

{% embed url="https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml" %}

{% embed url="https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/" %}

{% embed url="https://github.com/flannel-io/flannel" %}
