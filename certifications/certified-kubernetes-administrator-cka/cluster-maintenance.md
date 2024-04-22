# Cluster Maintenance

## OS Upgrades

When performing upgrades on worker nodes, It's best practice to:

* Drain the node `kubectl drain node01`
* When upgrade is completed, you must uncordon it to make it schedulable `kubectl uncordon node01`

#### If you only want to make the node unschedulable you can run:

```bash
kubectl cordon node01 # Make sure no pods are scheduled on node01
```

### Lab:

Everything went well.

## Kubernetes Releases

When you download a new Kubernetes release, it comes with all Kubernetes components with of same version, although some components like `etcd` and `coreDNS` come with different versions as they are separate projects.

{% embed url="https://github.com/kubernetes/kubernetes/releases" %}

## Cluster Upgrade Process

During an upgrade process to the cluster, here are important aspects to note, lets take v1.10 as an example:

#### If kube-apiserver is at v1.10, then:

* **controller-manager** must be v1.9 or v1.10
* **kube-scheduler** must be v1.9 or v1.10
* **kubelet** must be v1.8, v1.9 or v1.10
* **kube-proxy** must be v1.8, v1.9 or v1.10

This is not the case for **kubectl** as it can be v1.9, v1.10 or v1.11

These versions allows us to do live upgrade, component by component if required.

### When shall we upgrade?

Lets say we are at Kubernetes v1.10, we only have access to the 3 latest version, so if v1.13 is released, v1.10 will be unsupported. So before v1.13 release, it would be a great time to upgrade.

To upgrade, its recommended to upgrade one by one which means v1.10 then v1.11 then v1.12 then v1.13.

### Upgrading Process

The Kubernetes upgrade process is dependent on how the cluster is setup.

* If we deployed our cluster using AWS EKS, it can be done within a few clicks.
* If we deployed our cluster using kubeadm tool, it can be done within commands.
* If we deployed our cluster the Kubernetes hard way (From scratch), then we need to manually upgrade components ourselves

{% hint style="info" %}
We will be upgrading the cluster using kubeadm tool.
{% endhint %}

#### If we have cluster with master and worker nodes, and they are at v1.10, there are 2 major steps:

**1) Upgrade master nodes:** All management functions are down, although worker nodes still serve users

**2) Upgrade worker nodes**

#### Upgrading worker nodes has 3 strategies:

1\) Bring all nodes down then up (Has downtime)

2\) One node at a time

3\) Creating new nodes with newer versions and deleting old nodes (Convenient if on cloud)

### Kubeadm - Upgrade

#### We can see components versions and additional useful information if we run:

```bash
kubeadm upgrade plan
```

{% hint style="danger" %}
Remember, kubeadm doesn't install or upgrade kubelets, so we need to deal with it.
{% endhint %}

{% hint style="info" %}
Also, we need to upgrade the kubeadm tool itself before we perform the cluster upgrade.
{% endhint %}

#### Through this page in the k8s docs, you can upgrade the cluster step by step, starting with the control plane:

{% embed url="https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/" %}

I will share my experience when trying to set it up using the docs in the upcoming lab.

### Lab (IMPORTANT):

Now, I will discuss what happened through my lab, I ran `kubeadm upgrade plan` which appeared to me that my cluster version is v1.28.0 and got the target upgrade that I can upgrade to is v1.28.7 although the question asked to upgrade to exactly v1.29.0.

I had to change the package repository of kubernetes to v1.29 by editing `/etc/apt/sources.list.d/kubernetes.list` file.

#### After changing the Kubernetes package repository to v1.29, I ran:

```bash
sudo apt update
sudo apt-cache madison kubeadm
```

These commands gave me the versions I can upgrade my kubdeadm to which gave me `v1.29.0-1.1` which I wanted.

I proceeded and used the `=1.29.0-1.1` throughout the upgrade commands until I finished upgrading controlplane and uncordoned it.

After that, I had to upgrade worker nodes, the huge mistake I did is trying to run `kubectl` and `kubeadm` commands in worker node, it won't work as this is not a master node and doesn't have the components of controlplane.

#### So I had to:

```bash
kubectl drain node01 --ignore-daemonsets # Drain the worker node
ssh node01 # Enter the worker node
```

After entering the worker node, all I had to do is run the same commands as master node upgrade with the `=1.29.0-1.1` but I had to add one command before upgrading kubeadm which is `sudo kubeadm upgrade node` as shown in k8s worker node upgrade docs

#### For example:

```bash
sudo kubeadm upgrade node
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.29.0-1.1' && \
sudo apt-mark hold kubeadm
```

#### And then upgrade kubelet and kubectl:

{% code overflow="wrap" %}
```bash
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.29.0-1.1' kubectl='1.29.0-1.1' && \
sudo apt-mark hold kubelet kubectl
```
{% endcode %}

#### Then restart the kubelet daemon:

```bash
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

#### Now, our work is done, so we can exit the worker node and uncordon it from the control plane:

```bash
exit
kubectl uncordon node01
```

That's it!

## Backup & Restore Methods

### Backup - Resource Configs

#### One way to backup all services is to query kube-apiserver or use the kubectl utility:

```bash
kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml
```

{% hint style="warning" %}
This is only for few resources groups, there are other resource groups which must be considered
{% endhint %}

Some solutions such as [velero](https://velero.io/) take care of that for you.

### Backup - ETCD

All resources created within the cluster is stored in ETCD server, so its crucial to take a backup for it.

The ETCD server is hosted on master nodes, when configuring ETCD we specify a path where all data would be stored.

#### We could also take a snapshot from the ETCD server, which is a feature built-in in the ETCD server:

```bash
ETCDCTL_API=3 etcdctl snapshot save snapshot.db # Or full path (/var/snapshot.db)
```

```bash
ETCDCTL_API=3 etcdctl snapshot status snapshot.db # Status of backup
```

{% hint style="danger" %}
Remember to specify endpoints, cacert, etcd cert and key for authentication and access to the etcd cluster when using snapshot save.
{% endhint %}

### Restore - ETCD

#### To restore an ETCD snapshot, we must first stop the kube-apiserver because kube-apiserver depends on ETCD:

```bash
service kube-apiserver stop
```

{% code overflow="wrap" %}
```bash
ETCDCTL_API=3 etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup
```
{% endcode %}

#### Then we need to reload daemon and etcd services:

```bash
systemctl daemon-reload
service etcd restart
```

#### Finally, start the kube-apiserver:

```bash
service kube-apiserver start
```

{% hint style="info" %}
If using managed kubernetes cluster, you might not have access to etcd server, so resource backups using the kube-apiserver is better.
{% endhint %}

### Lab 1 (IMPORTANT):

#### First Restore the snapshot:

{% code overflow="wrap" %}
```sh
ETCDCTL_API=3 etcdctl snapshot restore /opt/snapshot-pre-boot.db --data-dir /var/lib/etcd-from-backup 
```
{% endcode %}

{% hint style="warning" %}
In this case, we are restoring the snapshot to a different directory but in the same server where we took the backup (the control plane node). As a result, the only required option for the restore command is the `--data-dir`
{% endhint %}

#### Next, update the `/etc/kubernetes/manifests/etcd.yaml`:

We have now restored the etcd snapshot to a new path on the controlplane - `/var/lib/etcd-from-backup`, so, the only change to be made in the YAML file, is to change the hostPath for the volume called `etcd-data` from old directory (`/var/lib/etcd`) to the new directory (`/var/lib/etcd-from-backup`).

With this change, `/var/lib/etcd` on the container points to `/var/lib/etcd-from-backup` on the `control plane` (which is what we want).

When this file is updated, the `ETCD` pod is automatically re-created as this is a static pod placed under the `/etc/kubernetes/manifests` directory.

> Note 1: As the ETCD pod has changed it will automatically restart, and also `kube-controller-manager`, `kube-apiserver` and `kube-scheduler`. Wait 1-2 to mins for this pods to restart. You can run the command: `watch "crictl ps | grep etcd"` to see when the ETCD pod is restarted.

> Note 2: If the etcd pod is not getting `Ready 1/1`, then restart it by `kubectl delete pod -n kube-system etcd-controlplane` and wait 1 minute.

> Note 3: This is the simplest way to make sure that ETCD uses the restored data after the ETCD pod is recreated. You don't have to change anything else.

{% hint style="warning" %}
**THIS STEP IS OPTIONAL AND NOT NEEDED FOR COMPLETING THE RESTORE**

If you do change `--data-dir` to `/var/lib/etcd-from-backup` in the ETCD YAML file, make sure that the `volumeMounts` for `etcd-data` is updated as well, with the mountPath pointing to `/var/lib/etcd-from-backup`
{% endhint %}

### Lab 2 (IMPORTANT):

#### Most important things to note in this lab:

* When switching to a new cluster and you want to check components pods, make sure to ssh to the controlplane of the cluster.
* When you can't see an etcd pod, it's most probably an external etcd and you must describe the kube-apiserver to see the external etcd IP, it must be mentioned there so you can ssh into it if needed.
* Running commands like `ps -aux | grep etcd` after ssh into the etcd server is crucial to get information regarding endpoints, data dirs, certs etc...
* Make sure to utilize the `scp` command to copy files from a node to another node specially when doing etcd snapshot restore and backup, examples shown below.
* Make sure to also utilize `chown` specially when restoring etcd snapshot.
* Make sure to edit the etcd service to update the data-dir after restoring, usually found at `/etc/systemd/system/etcd.service` path.

{% hint style="danger" %}
Never edit a static pod directly if you want to modify a data-dir for instance in etcd pod, always check the `/etc/kubernetes/manifests folder`, if not found then check the `/etc/systemd/system/etcd.service (External etcd).`

`kubelets always re-creates static pods if deleted. So to apply changes delete the static pod!`
{% endhint %}

#### On the `student-node`:

1. First set the context to `cluster1`:

```sh
student-node ~ ➜  kubectl config use-context cluster1
Switched to context "cluster1".
```

Next, inspect the endpoints and certificates used by the `etcd` pod. We will make use of these to take the backup.

{% code overflow="wrap" %}
```bash
student-node ~ ✖ kubectl describe  pods -n kube-system etcd-cluster1-controlplane  | grep advertise-client-urls
      --advertise-client-urls=https://10.1.218.16:2379

student-node ~ ➜  

student-node ~ ➜  kubectl describe  pods -n kube-system etcd-cluster1-controlplane  | grep pki
      --cert-file=/etc/kubernetes/pki/etcd/server.crt
      --key-file=/etc/kubernetes/pki/etcd/server.key
      --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
      --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
      --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
      --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
      /etc/kubernetes/pki/etcd from etcd-certs (rw)
    Path:          /etc/kubernetes/pki/etcd

student-node ~ ➜  
```
{% endcode %}

SSH to the `controlplane` node of `cluster1` and then take the backup using the endpoints and certificates we identified above:

{% code overflow="wrap" %}
```bash
cluster1-controlplane ~ ➜  ETCDCTL_API=3 etcdctl --endpoints=https://10.1.220.8:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot save /opt/cluster1.db
Snapshot saved at /opt/cluster1.db

cluster1-controlplane ~ ➜  
```
{% endcode %}

Finally, copy the backup to the `student-node`. To do this, go back to the `student-node` and use `scp` as shown below:

```sh
student-node ~ ➜  scp cluster1-controlplane:/opt/cluster1.db /opt
cluster1.db                                                                                                        100% 2088KB 112.3MB/s   00:00    

student-node ~ ➜ 
```

#### Step 1

Copy the snapshot file from the student-node to the etcd-server. In the example below, we are copying it to the /root directory:

```sh
student-node ~  scp /opt/cluster2.db etcd-server:/root
cluster2.db                                                                                                        100% 1108KB 178.5MB/s   00:00    

student-node ~ ➜  
```

#### Step 2

Restore the snapshot on the `cluster2`. Since we are restoring directly on the `etcd-server`, we can use the endpoint `https:/127.0.0.1`. Use the same certificates that were identified earlier. Make sure to use the `data-dir` as `/var/lib/etcd-data-new`:

```sh
etcd-server ~ ➜  ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/etcd/pki/ca.pem --cert=/etc/etcd/pki/etcd.pem --key=/etc/etcd/pki/etcd-key.pem snapshot restore /root/cluster2.db --data-dir /var/lib/etcd-data-new
{"level":"info","ts":1662004927.2399247,"caller":"snapshot/v3_snapshot.go:296","msg":"restoring snapshot","path":"/root/cluster2.db","wal-dir":"/var/lib/etcd-data-new/member/wal","data-dir":"/var/lib/etcd-data-new","snap-dir":"/var/lib/etcd-data-new/member/snap"}
{"level":"info","ts":1662004927.2584803,"caller":"membership/cluster.go:392","msg":"added member","cluster-id":"cdf818194e3a8c32","local-member-id":"0","added-peer-id":"8e9e05c52164694d","added-peer-peer-urls":["http://localhost:2380"]}
{"level":"info","ts":1662004927.264258,"caller":"snapshot/v3_snapshot.go:309","msg":"restored snapshot","path":"/root/cluster2.db","wal-dir":"/var/lib/etcd-data-new/member/wal","data-dir":"/var/lib/etcd-data-new","snap-dir":"/var/lib/etcd-data-new/member/snap"}

etcd-server ~ ➜  
```

#### Step 3

Update the systemd service unit file for `etcd`by running `vim /etc/systemd/system/etcd.service` and add the new value for `data-dir`:

```sh
[Unit]
Description=etcd key-value store
Documentation=https://github.com/etcd-io/etcd
After=network.target

[Service]
User=etcd
Type=notify
ExecStart=/usr/local/bin/etcd \
  --name etcd-server \
  --data-dir=/var/lib/etcd-data-new \
---End of Snippet---
```

#### Step 4

Make sure the permissions on the new directory is correct (should be owned by `etcd` user)

```sh
etcd-server /var/lib ➜  chown -R etcd:etcd /var/lib/etcd-data-new

etcd-server /var/lib ➜ 


etcd-server /var/lib ➜  ls -ld /var/lib/etcd-data-new/
drwx------ 3 etcd etcd 4096 Sep  1 02:41 /var/lib/etcd-data-new/
etcd-server /var/lib ➜ 
```

#### Step 5

Finally, reload and restart the etcd service.

```sh
etcd-server ~ ➜  systemctl daemon-reload 
etcd-server ~ ➜  systemctl restart etcd
etcd-server ~ ➜  
```

### Working with ETCDCTL

`etcdctl` is a command line client for [**etcd**](https://github.com/coreos/etcd).

In all our Kubernetes Hands-on labs, the ETCD key-value database is deployed as a static pod on the master. The version used is v3.

To make use of etcdctl for tasks such as back up and restore, make sure that you set the ETCDCTL\_API to 3.

You can do this by exporting the variable ETCDCTL\_API prior to using the etcdctl client.

#### This can be done as follows:

```bash
export ETCDCTL_API=3
```

To see all the options for a specific sub-command, make use of the **-h or --help** flag.

#### Example:

```bash
ETCDCTL_API=3 etcdctl snapshot save -h
ETCDCTL_API=3 etcdctl snapshot restore -h
```

## Slides
