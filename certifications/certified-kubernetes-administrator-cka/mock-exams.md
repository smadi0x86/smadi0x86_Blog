# Mock Exams

## Mock Exam 1

94%, only one question wrong which is JSONPATH

{% hint style="info" %}
Make sure to check services creation using imperative commands as I had some confusion and I don't know really how I got them correctly
{% endhint %}

### Second Attempt

100%

## Mock Exam 2

43%, 4 questions wrong, which are about:

*   A pod definition file is created at `/root/CKA/use-pv.yaml`. Make use of this manifest file and mount the persistent volume called `pv-1`. Ensure the pod is running and the PV is bound.

    mountPath: `/data` persistentVolumeClaim Name: my-pvc
* Create a new user called `john`. Grant him access to the cluster. John should have permission to `create, list, get, update and delete pods` in the `development` namespace . The private key exists in the location: `/root/CKA/john.key` and csr at `/root/CKA/john.csr`. `Important Note`: As of kubernetes 1.19, the CertificateSigningRequest object expects a `signerName`.
* Create a nginx pod called `nginx-resolver` using image `nginx`, expose it internally with a service called `nginx-resolver-service`. Test that you are able to look up the service and pod names from within the cluster. Use the image: `busybox:1.28` for dns lookup. Record results in `/root/CKA/nginx.svc` and `/root/CKA/nginx.pod`
*   Create a static pod on `node01` called `nginx-critical` with image `nginx` and make sure that it is recreated/restarted automatically in case of a failure.

    Use `/etc/kubernetes/manifests` as the Static Pod path for example.

### Second Attempt

#### 85%, only wrong question was:

* Create a nginx pod called `nginx-resolver` using image `nginx`, expose it internally with a service called `nginx-resolver-service`. Test that you are able to look up the service and pod names from within the cluster. Use the image: `busybox:1.28` for dns lookup. Record results in `/root/CKA/nginx.svc` and `/root/CKA/nginx.pod`

I didn't nslookup on the service, just nslookup'd the pod

### Third Attempt

Same issue as before when resolving services, we must use the service name `nslookup <service-name>`

## Mock Exam 3

#### Wrong:

Q1, Q2, Q5

### Second Attempt

#### So important notes:

* In network policy, make sure to note how it was done, it has some serious confusion (I had to get back to the video solution for it.

We have deployed a new pod called `np-test-1` and a service called `np-test-service`. Incoming connections to this service are not working. Troubleshoot and fix it.\
Create NetworkPolicy, by the name `ingress-to-nptest` that allows incoming connections to the service over port `80`.

Important: Don't delete any current objects deployed.
