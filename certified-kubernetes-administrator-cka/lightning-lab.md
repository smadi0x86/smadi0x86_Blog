# Lightning Lab

This environment is valid for `60 minutes`, challenge yourself and try to complete all `5-8 questions` within `30 minutes`.

You can toggle between the questions but make sure that you click on `END EXAM` before the timer runs out. To pass, you need to secure **80%**.

## Wrong Questions: First Attempt

### Q1

Create a pod called `secret-1401` in the `admin1401` namespace using the `busybox` image. The container within the pod should be called `secret-admin` and should sleep for `4800` seconds.

The container should mount a `read-only` secret volume called `secret-volume` at the path `/etc/secret-volume`. The secret being mounted has already been created for you and is called `dotfile-secret`

{% hint style="info" %}
I didn't create a PVC
{% endhint %}

### Q2

A new deployment called `alpha-mysql` has been deployed in the `alpha` namespace. However, the pods are not running. Troubleshoot and fix the issue. The deployment should make use of the persistent volume `alpha-pv` to be mounted at `/var/lib/mysql` and should use the environment variable `MYSQL_ALLOW_EMPTY_PASSWORD=1` to make use of an empty root password.

Important: Do not alter the persistent volume.

{% hint style="info" %}
I created a volume instead of a secret volume, it differs
{% endhint %}

### Q3

Print the names of all deployments in the `admin2406` namespace in the following format:\
\
`DEPLOYMENT CONTAINER_IMAGE READY_REPLICAS NAMESPACE`\
\
`<deployment name> <container image used> <ready replica count> <Namespace>`. The data should be sorted by the increasing order of the `deployment name`.

Example:\
\
`DEPLOYMENT CONTAINER_IMAGE READY_REPLICAS NAMESPACE`\
`deploy0 nginx:alpine 1 admin2406`\
Write the result to the file `/opt/admin2406_data`.

{% hint style="info" %}
I didn't practice json path
{% endhint %}

{% code overflow="wrap" %}
```
kubectl -n admin2406 get deployment -o custom-columns=DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[].image,READY_REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace --sort-by=.metadata.name > /opt/admin2406_data
```
{% endcode %}

## Second Attempt

2 Wrong, the jsonpath and the deployment alpha-mysql.

* Regarding the jsonpath I don't know what to do tbh
* For the deployment I must create a new PVC based on the information of the PV, what I was doing is trying to use a current PVC and change the deployment based on it which is not ideal.

## Third Attempt
