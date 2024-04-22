# Application Lifecycle Management

## Rolling Updates & Rollbacks

A new rollout creates a new deployment revision (Revision 1), When we upgrade the application, a new rollout is triggered and a new deployment is created (Revision 2), this allows us to rollout to previous revision if anything breaks.

```bash
kubectl rollout status deployment <deployment-name>  # Current rollout
kubectl rollout history deployment <deployment-name> # History of rollouts
```

### Deployment Strategies

#### Recreate strategy

We bring all pods of our application down, then spin the updated ones after. This causes downtime which is bad.

#### Rolling Update (Default)

We bring each pod one by one, and whenever we get an older version pod down, a newer version pod spins up.

To update a deployment, you could vim into the deployment and change image version, what happens under the hood is that the deployment creates a new replicaset (replicaset-2) with the new image version and also start removing pods from the original replicaset (replicaset-1).

{% code overflow="wrap" %}
```bash
kubectl apply -f deployment-def.yml # After updating using vim
kubectl set image deployment <deployment-name> nginx=nginx:1.9.1 # This won't change version in the yaml definition
```
{% endcode %}

#### If something went wrong when you updated your deployment you can run:

```bash
kubectl rollout undo deployment <deployment-name>
```

### Lab:

Just a small mistake was identified, when changing deployment from RollingUpdate to Recreate, there will be additional configuration specified for RollingUpdate so make sure to also remove them and only keep the `type: Recreate`.

## Commands & Arguments

#### We can specify docker images commands and arguments overrides in the pod definitions:

### Lab (Review):

I had issues with the pod definition of command and arguments, also I had misunderstanding of some concepts, here are the correct ones:

* Command in pod definition is same as Entrypoint in Dockerfile
* args in pod definition is same as CMD in Dockerfile

Instead of deleting the pod and re-applying the /tmp/ pod, we can run `kubectl replace --force -f /tmp/...`

## Configuring Environment Variables

#### To set environment variables in a pod definition:

## Configmaps

Instead of defining key-value pairs environment variables in the pod definition, we could create configmaps.

When a pod is created we inject the configmap into the pod.

* Create configmap
* Inject configmap to pod

### Create configmap

#### Creating configmap using imperative commands:

{% code overflow="wrap" %}
```bash
kubectl create configmap <config-name> -from-literal=APP_COLOR=blue --from-literal=APP_MOD=prod

# Or

kubectl create configmap <config-name> --from-file=.env
```
{% endcode %}

#### Creating configmap using declarative approach:

```
kubectl create -f config-map.yaml
```

```
kubectl get configmaps
kubectl describe configmap
```

### Inject a configmap to a pod

We add a property under containers called `envFrom,` note that this is a pod yaml that we still didn't create/apply (not for reference).

#### Ways to inject a configmap:

* ENV
* SINGLE ENV
* VOLUME

### Lab (Review):

A huge issue I encountered is the syntax of configmaps, I was so confused what to use and had to look at the solution.

The main issue here is that I didn't look correctly in the k8s docs, I must check links found at bottom of pages If I didn't find anything useful in a configmap docs page.

Also I didn't utilize the `--force` flag in the `kubectl replace` command.

## Configure Secrets

#### Two steps are required to work with secrets:

* Create Secret
* Inject Secret

### Create Secret

#### Using imperative command:

```bash
kubectl create secret generic <secret-name> --from-literal=<key>=<value>
kubectl create secret generic <secret-name> --from-file=<secret-file>
```

#### Using Declarative approach:

```bash
echo -n "mysqlpassword" | base64 # DB_Password encoded string
```

```bash
kubectl create -f secret-data.yaml
kubectl get secrets
```

### Inject Secret

#### Other ways to inject secrets:

* ENV
* SINGLE ENV
* VOLUME

{% hint style="danger" %}
Secrets are not encrypted. only encoded. Secrets aren't encrypted in ETCD. Make sure to not push them to a git. Anyone able to create pods/deployments in same namespace can access secrets.
{% endhint %}

{% hint style="success" %}
[Enable Encryption at rest for ETCD](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/). Configure least-privilege access to secrets (RBAC). Consider 3rd party secrets store providers such as AWS, Vault etc...
{% endhint %}

#### Also the way Kubernetes handles secrets. Such as:

* A secret is only sent to a node if a pod on that node requires it.
* Kubelet stores the secret into a tmpfs so that the secret is not written to disk storage.
* Once the Pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.

Read about the [protections ](https://kubernetes.io/docs/concepts/configuration/secret/#protections)and [risks](https://kubernetes.io/docs/concepts/configuration/secret/#risks) of using secrets [here](https://kubernetes.io/docs/concepts/configuration/secret/#risks)

Having said that, there are other better ways of handling sensitive data like passwords in Kubernetes, such as using tools like Helm Secrets, [HashiCorp Vault](https://www.vaultproject.io/).

### Lab (Review):

Didn't include the `generic` option when creating the secret `kubectl create secret generic ...` and I found it by using `kubectl create secret -h`

There are alot of mistakes identified, I did the same mistake of not checking the docs well, I was asked to load secrets as environment variables, so I must have went:

And I must have used `envFrom` which takes environment variables from a pod

{% hint style="danger" %}
Also, regarding vim, to paste correctly in vim, I must copy the spaces too from the docs, also paste from the beginning of the line so it can be indented correctly.
{% endhint %}

## Encrypting Secret Data At Rest

#### Follow this docs page for additional information:

{% embed url="https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/#encrypting-your-data" %}

## Multi Container Pods

When building web applications, you might want to have a logging agent as a container separate from your app container, in that case you can do a multi container pod, that can contain more than one container and share same storage and network that they can refer to each other as `localhost`

#### To create a multi-container pod, define it as an array under containers section:

### Lab:

Everything went well except I had to concentrate on the correct namespace for editing the pod.

{% hint style="danger" %}
I was editing the app pod in the default namespace instead of the elastic-stack namespace.
{% endhint %}

#### I tried to exec to the pod but it wasn't working, I didn't specify the `-n` `-it` flags:

{% code overflow="wrap" %}
```bash
kubectl -n elastic-stack exec -it app -- /bin/bash # Or instead of /bin/bash run the command you want (cat /log/app.log)
```
{% endcode %}

#### Docs used:

{% embed url="https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/" %}

## InitContainers

In Kubernetes, multi-container pods use `initContainers` for tasks that must complete before the main containers start. These are specified in the pod configuration.

#### Here's a shortened example:

{% code overflow="wrap" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```
{% endcode %}

In this configuration, `initContainers` run tasks sequentially before the main container starts. If an `initContainer` fails, Kubernetes restarts the pod until it succeeds.

#### Read more about InitContainers here:

[https://kubernetes.io/docs/concepts/workloads/pods/init-containers/](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)

### Lab:

#### Its as usual a `kubectl edit pod` yaml issue, the `- name` is always first:

Also, I should have checked pod logs before fixing the issue to fix it.

## Slides
