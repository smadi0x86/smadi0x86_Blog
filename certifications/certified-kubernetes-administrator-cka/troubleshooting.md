# Troubleshooting

## Application Failure

This section is straight forward, to debug applications refer to the kubernetes docs page.

{% embed url="https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/#container-exec" %}

### Lab:

#### The most things I mistaked in:

* Not paying attention to the svc names

#### The troubleshooting went in these processes:

* kubectl get pods
* kubectl describe pods
* kubectl get svc
* kubectl describe svc
* kubectl get deployments

## Control Plane Failure

#### First check status of nodes if they are healthy:

```bash
kubectl get nodes
```

#### Then status of pods:

```
kubectl get pods
kubectl get pods -n kube-system
```

#### You can also check the control plane services manually if they are deployed as services:

```bash
service kube-apiserver status
service kube-controller-manager status
service kube-scheduler status
service kubelet status
service kube-proxy status
```

#### Check Service logs:

```bash
kubectl logs kube-apiserver-master -n kube-system # If deployed as pods
sudo journalctl -u kube-apiserver # If deployed as services
```

{% embed url="https://kubernetes.io/docs/tasks/debug/debug-cluster/" %}

### Lab:

The issue I encountered is so important, when I checked the logs of kube-controller-manager, there was a file not found error, but I made sure the client certificate path was right.

{% hint style="warning" %}
Turns out that the defnition files have volumeMounts and paths, these paths are given so they can be used in the configration above, I didn't check them and this was a huge problem.

The solution was that the `hostPath` with the value `/etc/kubernetes/pki` was changed and I had to revert it back to `/etc/kubernetes/pki`
{% endhint %}

## Worker Node Failure

Same steps as before, describe the worker nodes to lead us to the solution.

#### When a worker node stops communicating to a master it is shown as unknown:

#### Then we can proceed to check status of the nodes:

```bash
top # Compute Information
df -h # Disk Space Information
```

#### Also checking kubelet status on the worker nodes:

```bash
service kubelet status
sudo journalctl -u kubelet
```

#### Check Certificates:

```bash
openssl x509 -in /var/lib/kubelet/worker-1.crt -text
```

### Lab:

Everything went well, but make sure to view the certificate details from the below docs

{% embed url="https://kubernetes.io/docs/tasks/administer-cluster/certificates/" %}

#### Also, when editing the /etc/kubernetes/kubelet.conf make sure to restart the kubelet using:

```bash
systemctl restart kubelet
```

## Network Troubleshooting

### CoreDNS Troubleshooting Commands

#### **Check if a network plugin is installed**:

```sh
kubectl get pods -n kube-system
```

#### **Upgrade Docker** (specific commands can vary based on your system):

```sh
# For example, on Ubuntu:
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

#### **Disable SELinux**:

```sh
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

#### **Modify CoreDNS to allow privilege escalation**:

```sh
kubectl -n kube-system get deployment coredns -o yaml | \
sed 's/allowPrivilegeEscalation: false/allowPrivilegeEscalation: true/g' | \
kubectl apply -f -
```

**Adjust kubelet config to use an alternate resolv.conf**

#### Edit the kubelet config file (usually at `/var/lib/kubelet/config.yaml`) and add:

```yaml
resolvConf: /path-to-your-real-resolv-conf
```

Then restart the kubelet service.

#### **Edit Corefile** to forward DNS queries directly to an upstream DNS:

```sh
kubectl -n kube-system edit configmap coredns
# Then replace "forward . /etc/resolv.conf" with "forward . 8.8.8.8"
```

#### **Check kube-dns service endpoints**:

```sh
kubectl -n kube-system get ep kube-dns
```

### Kube-Proxy Troubleshooting Commands

#### **Check kube-proxy pod status**:

```sh
kubectl get pods -n kube-system | grep kube-proxy
```

#### **Check kube-proxy logs**:

```sh
kubectl logs <kube-proxy-pod-name> -n kube-system
```

#### **Verify kube-proxy configmap**:

```sh
kubectl get configmap -n kube-system | grep kube-proxy
```

#### **Check for kube-proxy network bindings**:

```sh
netstat -plan | grep kube-proxy
```

#### Debug Service issues:

{% embed url="https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/" %}

#### DNS Troubleshooting:

{% embed url="https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/" %}

### Lab:

First, I must check if CNI is installed, and I falied to do so, its as easy as just running a `$ kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml`

Also, second challenge I were so close, I knew the error was from kube-proxy config not found error, the same mistake I did was not checking the volumeMounts when editing the pod, its not necessary that the path on the pod must be in the current live machine of where you're solving the challenges.

## Slides
