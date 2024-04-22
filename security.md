# Security

## Authentication

#### We have 2 types of users:

* **Users:** Admins & Developers
* **Bots:** Service Accounts which are other processes, services, apps that require access to cluster

### Users

#### Kubernetes doesn't manage user accounts natively, it relies on external sources such as:

* File with user details
* Certificates
* 3rd-party identity service like LDAP

So you cannot create users or list users in a Kubernetes cluster

### Service Accounts

Kubernetes can manage service accounts using the kubeapi-server

### Authentication Mechanisms

#### How does kube-apiserver authenticate?

* **Static Password File:** List of username & password in a file
* **Static Token File:** Usernames & Token in a file
* **Certificates:** Authenticate using certificates
* **Identity Services:** Connect to 3rd party authentication protocols such as LDAP, Kerberos etc...

#### Basic Authentication

* Static Password File
* Static Token File

We can create list of users with their passwords as a csv file:

Then pass the csv file name as an option to the kube-apiserver then restart manually

Or using the kubeadm tool in the `/etc/kubernetes/manifests/kube-apiserver.yaml` file which will be restarted automatically by the kubeadm tool

To authenticate user using the basic credentials while accessing the kube-apiserver, specify the user and password

We can have a 4th column to specify a group

Also, if we are using tokens instead of password, we must add it to the csv file and use the token as bearer in the request header

{% hint style="danger" %}
This is not the recommended approach for authentication as it stores plain text information

Consider volume mount while providing the auth file in a kubdeadm setup
{% endhint %}

## TLS Basics

#### A certificate is used to guarantee trust between two parties during a transaction, for example:

If user tries to access a web server, TLS certificates ensure the traffic between user and web server is encrypted and the web server is who it says it is.

{% hint style="info" %}
Hackers can sniff symmetric keys sent by user to the server and decrypt the messages
{% endhint %}

To solve this issue, we use asymmetric encryption to generate public and private keys using OpenSSL

#### Let's view what happens:

* User requests the web server on HTTPS
* The web server sends a public key to the user
* The user browser encrypts the symmetric key using the public key sent by the webserver
* The user then sends his message (encrypted with symmetric key) which is also encrypted with public key of the server
* The server receives the message and decrypts it using its private key

The hacker only has a public key and encrypted data which he can't do anything with them.

{% hint style="info" %}
Hackers now know our trick and creates their own server and generating their own private and public keys for a secure connection and somehow routes our request to their server
{% endhint %}

To solve this issue, here is where the certificates play a crucial role, so when we request to the server on HTTPS and receive its public key.

#### We can inspect the public key and see its certificate which includes information like:

* Public key of server
* Location of server
* Who is the certificate for
* DNS Information etc...

{% hint style="warning" %}
Most important part to verify a certificate is to check who signed and issued the certificate
{% endhint %}

{% hint style="info" %}
When you generate a certificate yourself, it is a self-signed certificate which appears to be suspicious, browsers validates and warns you if the certificate is fake/self-signed
{% endhint %}

### How do we get our certificates to be signed by an authority?

#### That's where Certificate Authorities (CA) comes into play, some popular authorities includes:

* Symantec
* Comodo
* GlobalSign
* digicert

#### The process of signing a certificate by an authority is:

* Generate a certificate signing request (CSR) using key we generated earlier and name of our website
* Authority validates the information you sent
* Certificate is signed and sent back to you

{% hint style="info" %}
You now have a certificate signed by a CA that the browsers trust

CAs use different techniques to make sure you're the actual owner of that domain

All CAs have their own set of public and private keys which are built in to the browsers

Browser uses public key of CA to check if its signed by CA itself
{% endhint %}

#### Let's say we have sites hosted privately for our organization, what shall we do?

CAs offers private hosting of their servers, we can deploy the CA server internally and then have the public key of out internal CA server installed on all employees browsers and establish secure connectivity in the organization.

#### In summary:

* Admin generates a key pair for securing SSH
* Web server generates a key pair for securing the website with HTTPS
* CA generates its own key pairs to sign certificates
* End users generates a single symmetric key to encrypt his credentials and uses it after establishing trust to the webserver to send his credentials for authentication

{% hint style="info" %}
All of these key pair generations mentioned are called Public Key Infrastructure (PKI)

You can encrypt data with both public and private keys but only decrypt with private key
{% endhint %}

### Naming Conventions

#### server.crt = server public key etc...

#### server.key = server private key etc...

## TLS in Kubernetes

Communication between Master and worker nodes must be secure, also when an admin uses the kubectl utility to communicate with kube-apiserver it must be secure. Overall all communications between Kubernetes components must be secure.

#### So the 2 primary requirements are:

* Server certificates for servers
* Client certificates for clients

Let's start with the server components which uses TLS certificates.

### Server Certificates for servers

{% hint style="warning" %}
Server private and public key naming convention may differ depending on who and how the cluster was setup
{% endhint %}

#### KubeAPI server

Lets start with kube-apiserver which exposes an https service that other components and external users use to manage the Kubernetes cluster.

#### ETCD server

#### Kubelet server

### Client Certificates for clients

Clients are admins or components who needs access to the kube-apiserver through kubectl or rest API.

#### Admins

Lets start with the admin which must have key-pairs to talk to the kube-apiserver and authenticate

#### Scheduler

The scheduler needs to communicate with the kube-apiserver to get pods who needs scheduling, which it must have key-pairs to authenticate

#### Kube Controller Manager

#### Kube Proxy

#### This picture below summarizes everything regarding certificates for both client and server:

## Certificate Generation

#### To generate certificates we must have at least one CA in our cluster, in fact we can have more than one:

* CA for server
* CA for client

For now, we'll just stick to one CA for our cluster.

We will use OpenSSL utility to generate our certificates

#### Generate key:

```bash
openssl genrsa -out ca.key 2048 # ca.key
```

#### Certificate Signing Request:

```bash
openssl -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
```

#### Sign Certificates:

```bash
openssl x509 -req -in ca.csr -signkey ca.key -out ca.csr # Self-signed
```

To generate client certificates starting with admin

#### Generate key:

```bash
openssl genrsa -out admin.key 2048
```

#### Certificate Signing Request:

{% code overflow="wrap" %}
```bash
openssl req -new -key admin.key -subj "CN=kube-admin/OU=system:masters" -out admin.csr
```
{% endcode %}

{% hint style="success" %}
To distinguish admins from normal users, we must put the admin user in a group, and mention it in the Certificate Signing Request as `/OU=<group-name>`
{% endhint %}

{% hint style="danger" %}
For client system components such as scheduler/controller manager/kube-proxy, we must include a prefix to their certificate name (CN) like `"CN=SYSTEM:kube-scheduler"`
{% endhint %}

{% hint style="warning" %}
CN=kube-admin doesn't have to be kube-admin, you can name it as you like, but remember this is the name that kubectl client authenticates with and can be found in logs and elsewhere
{% endhint %}

#### Sign Certificates:

{% code overflow="wrap" %}
```bash
openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt # admin.crt is the certificate that will be used to authenticate to kubernetes cluster
```
{% endcode %}

#### Now, instead of authenticating with username and password/token, we can use the kube-admin certificate we generated and put it in the `kubeconfig` yaml to authenticate with it:

#### Or using curl (not recommended):

### Peer Certificates

Lets take ETCD server as an example, ETCD can be deployed as a cluster across multiple server in a high availability environment, so we need to generate peer certificates to secure communication between different ETCD members in the cluster.

{% hint style="danger" %}
ETCD config requires the CA root certificate to verify the client connecting to ETCD are valid
{% endhint %}

### KubeAPI server

#### Some common names given to kube-apiserver are:

* kubernetes
* kubernetes.default
* kubernetes.default.svc
* kubernetes.default.svc.cluster.local
* By its IP address

{% hint style="info" %}
Those referring to the kube-apiserver by these names can establish a valid connection
{% endhint %}

To generate a certificate to the kube-apiserver

```bash
openssl genrsa -out apiserver.key 2048
```

To consider all alternative names to offer flexibility in establishing connection to kube-apiserver, create an `openssl.cnf` file

Then pass it as a config when generating the certificate signing request

{% code overflow="wrap" %}
```bash
openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" -out apiserver.csr -config openssl.cnf
```
{% endcode %}

```bash
openssl x509 -req -in apiserver.csr -CA ca.crt -CAkey ca.key -out apiserver.crt
```

Specify these keys in the kube-apiserver service configuration

### Kubelets

Certificates created for kubelet are named based on the node they are on.

We also mentioned a set of client certificates that will be used by kubelet to authenticate to kube-apiserver. The kube-apiserver needs to know which node is authenticating (use prefix of node name) to give it right set of permissions using the group.

Once certificates are generated they go to `kubeconfig` file.

## View Certificate Details

#### To check certificate details, lets start with kube-apiserver as an example:

View the kube-apiserver static pod definition found in `/etc/kubernetes/manifests/kube-apiserver/yaml`

Then to inspect a certain certificate, use the `openssl x509` command

```bash
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

{% hint style="warning" %}
Certificate requirements are found in Kubernetes docs in details
{% endhint %}

{% embed url="https://kubernetes.io/docs/setup/best-practices/certificates/" %}

#### When you run into issues, you want to start looking at the service logs:

```bash
kubectl logs etcd-master
```

#### Sometimes if the core components such as kubeapi-server/etcd-server is down, kubectl won't function, so you need to go one level down to docker:

```bash
docker ps -a
docker logs <container-id>
```

### Lab:

I was asked to give the CN Name of kubeAPI server certificate, I chose the Issuer CN instead of the Subject CN which is wrong.

#### Most used command:

```bash
openssl x509 -in <cert-path> -text -noout # Get certificate information
```

Checking the certificate paths and its name if its correct or not, then check the static pod definition to fix the issue.

Utilize `crictl` to troubleshoot if kubectl is not working due to kube-apiserver or etcd server are down.

```bash
crictl ps -a # | grep kubeapi if needed
crictl logs <container-id>
```

It appeared to be the wrong path of `--etcd-cafile`, which must be `/etc/kubernetes/pki/etcd/ca.crt`

## Certificates API

The CA is whatever server that has the key pairs which we can sign certificates with, in our case its the master node.

Whenever a certificate expires, we need to do same steps to re-generate them, this can be inefficient as we scale, so Kubernetes helps us by providing a certificate API.

#### Instead of manually entering the master node and signing the certificates ourselves, we could:

* Create CertificateSigningRequest object
* Review requests
* Approve requests
* Share certificates to users

#### Lets take an example:

* A user creates a key and then generates a certificate signing request using the key with his name on it
* Then users sends this request to the admin
* Admin takes the key and creates a CertificateSigningRequest object using yaml definition

{% hint style="danger" %}
The request must be encoded with base64 before adding it to the request in definition yaml using `cat jane.csr | base64`
{% endhint %}

* Once the object is created, admin can see it using `kubectl get csr`
* Admin can then approve the csr using `kubectl certificate approve jane`
* Admin then `kubectl get csr jane -o yaml` and decode it using `echo "encoded-cert>" | base64 -d` and share it with the end user

{% hint style="info" %}
All of these tasks are done by the **csr-approving** and **csr-signing controllers** in the controller manager component
{% endhint %}

### Lab:

The main issue here is doing the CertificateSigningRequest object, specially that in docs it was given as a cat command.

#### Also when encoding the csr, by default base64 prints it line by line, to make it work use this command:

```bash
cat akshay.csr | base64 -w 0
```

{% hint style="warning" %}
Make sure to utilize `dd` for delete entire line and `u` for undo in vim, also to copy the whole base64 output with the "`=`"
{% endhint %}

## Kubeconfig

Kubeconfig is set by default in the `$HOME/.kube/config` directory and kubectl uses it without you specifying the config path with the command you enter.

#### To create a kubeconfig, it can be created using a pod definition yaml as shown below:

{% hint style="warning" %}
Regarding certificates, you must specify full path, also you could use `certificate-authority-data` instead to paste the base64 encoded certificate.
{% endhint %}

#### To view current context (The user and cluster you are using):

```
kubectl config current-context
```

#### To view all the config:

```bash
kubectl config view
kubectl config view --kubeconfig=custom-config
```

#### To use another context:

```bash
kubectl config use-context prod-user@production # use new cluster and user
kubectl config use-context prod-user # use new user, samse cluster
kubectl config use-context production # use new cluster, same user

kubectl config -h # Additional commands
```

#### You also can add a namespace in the context definition yaml, so you can switch to a new context with the namespace you added:

### Lab:

#### The kubeconfig context definition in the yaml already have a name which specifies what cluster we must access and with what user, so if we want to change and use the research current context we should run:

```bash
kubectl config use-context research
```

{% embed url="https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/" %}

## API Groups

#### Core API Groups:

#### Named API groups:

{% hint style="danger" %}
You won't be allowed to curl like that, so you need to use your credentials as parameters to authenticate to the API

As an alternative, you could start a `kubectl proxy` that uses your credentials from kubeconfig and then `curl localhost:8001`
{% endhint %}

### Key Takeaways

All resources in Kubernetes are grouped into different API groups

{% hint style="info" %}
We can use all of these APIs to allow and deny actions for authorization, specially in the RBAC yaml definitions in`apiGroup:[]`
{% endhint %}

## Authorization

#### There are different authorization mechanisms in Kubernetes:

* Node Authorization
* Attribute Based Authorization (ABAC)
* Role Based Authorization (RBAC)
* Webhook

### Node Authorization

Node authorization is a special-purpose authorization mode that specifically authorizes API requests made by kubelets.

In Kubernetes Node Authorization, a "user" refers to a system component, such as a kubelet, rather than a human operator.

The kubelet acts on behalf of a node, making requests to the KubeAPI server. The Node Authorizer processes these requests, determining access based on the node's identity and the resources it needs.

* Kubelets are registered as "users" within the SYSTEM:NODE group.
* Each kubelet's username is prefixed with SYSTEM:NODE, distinguishing their identity for authorization purposes.

This framework ensures that kubelets requests are appropriately authorized by the node authorizer.

{% embed url="https://kubernetes.io/docs/reference/access-authn-authz/node/#kubelets-with-undifferentiated-usernames" %}

### ABAC

ABAC is meant for external access to the kube-apiserver which are for instance admins/developers.

#### ABAC is to associate a user or a group of users a set of permissions by creating a permission file in a json format and passing it to the kube-apiserver:

{% hint style="danger" %}
Every time you need to add/change policies you must edit this policy file manually and restart kubeapi-server, which is not efficient and difficult to manage.
{% endhint %}

### RBAC

Instead of directly assigning a policy to a user or group of users, we define a role (for developers in our case) and then assign all developers for this role.

So we only need to modify the role instead of modifying every single policy file.

### Webhook

For outsourcing the authorization mechanism, we could use 3rd party tool like Open Policy Agent.

Kubernetes make an API call to Open Policy Agent with information about user and his access requirements and let Open Policy Agent decide if he's permitted or not.

Based on the Open Policy Agent response, the user is granted access.

### Authorization Modes

The authorization mode is a configuration parameter option specified in the kube-apiserver configuration.

We didn't mention 2 Authorization mechanisms which are `AlwaysAllow` and `AlwaysDeny`

{% hint style="danger" %}
By default, the authorization mode is set to `AlwaysAllow`
{% endhint %}

{% hint style="success" %}
When we specify more than one mechanism, they will get executed by order.

If Node authorizer fails to grant permission, it will move to RBAC, if RBAC grants the permission it returns to user and webhook authorization is not used.
{% endhint %}

## Role Based Access Controls

#### We can create an RBAC using an object definition yaml:

#### Next step is to link user to that role, which we must create a Role Binding object definition:

{% hint style="info" %}
You can add the namespace under metadata, by default its the default namespace
{% endhint %}

#### To view all roles and role bindings:

```bash
kubectl get roles
kubectl get rolebindings
```

#### To check our current access, we can run:

```
kubectl auth can-i create deployments
yes
kubectl auth can-i delete nodes
no
etc...
```

#### If you're an admin and want to check your users permissions, you can:

```bash
kubectl auth can-i create deployments --as dev-user
no
kubectl auth can-i delete nodes --as dev-user --namespace test # In namespace test
yes
```

### Lab:

When adding additional rules (specially different api groups such as apps, storage etc...)

#### To get all resources information which is so useful for RBAC:

```bash
kubectl api-resources # Utilize grep to look for certain resources
```

{% embed url="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#-strong-api-groups-strong-" %}

## Cluster Roles & Role Bindings

#### There are two types of resources in kubernetes:

* **Namespaced:** Within a namespace, usually kube-system for system components and default for normal resources such as pods, deployments.
* **Cluster Scoped:** Within the whole cluster and they are not in a namespace.

#### To view all resources that are namespaced:

{% code overflow="wrap" %}
```bash
kubectl api-resources --namespaced=true # --namespaced=false if you want cluster scoped resources
```
{% endcode %}

### Cluster Roles

They are regular roles but only for cluster scoped resources.

{% hint style="info" %}
In Kubernetes docs, you can view how to create cluster roles and regular roles using kubectl
{% endhint %}

{% embed url="https://kubernetes.io/docs/reference/access-authn-authz/rbac/#kubectl-create-clusterrole" %}

### Lab:

Everything went well, the most useful thing I wasn't utilizing is:

```
kubectl api-resources
```

Also, make sure to check if the APIVERSION, if its v1 then in the definition yaml make sure the `apiGroup: [""]` is written like this, if its a deployment (apps) then `apiGroup: ["apps]`

## Service Accounts

#### There are 2 types of accounts in kubernetes:

* User accounts: Are used by humans, Administrators for management or developers for deploy an application
* Service accounts: Are used by bots, for example Prometheus to gather metrics for alerts and visualize it on a dashboard by quertying the kube-apiserver (needs authentication)

#### To create service accounts:

```bash
kubectl create serviceaccount prometheus-sa
kubectl get serviceaccount
```

{% hint style="info" %}
When creating a service account, a token is generated by default automatically which can be used by external application while authenticating to the kube-apiserver as a bearer token
{% endhint %}

#### The token created by the service account is in fact a secret which can be viewed by running:

```bash
kubectl get secrets
kubectl describe secret prometheus-sa-token
```

A new update to serviceaccounts was introduced which changed how it works, a TokenRequest API was introducted which now offers JWT expiration and other security enhancements, the old way of serviceaccounts tokens didn't have an expiration date and they were created automatically which is not the case now.

```
kubectl create serviceaccount prometheus-sa
```

#### And to create a token as its not generated automatically:

```
kubectl create token prometheus-sa
```

#### Although post v1.24 you can still create a non-expiry token using a secret object definition, but this is not recommended at all and if you did it, make sure to create the service account first.

### Lab:

#### I had to concentrate on how to include a serviceaccount in a deployment definition:

#### Also, I could have used:

```bash
kubectl set serviceaccount deployment web-dashboard dashboard-sa
```

## Image Security

#### When specifying images in pod object definition yaml such as `image: nginx` it is in fact like this behind the scenes:

You may have your own set of images that are stored in your private repository, whether its docker hub, ECR or other registries.

#### When an image is private and you want to pull it, you must authenticate to the registry first, this can be done in kubernetes as follows:

* We create a secret object with our registry URL, username and password

{% code overflow="wrap" %}
```bash
kubectl create secret docker-registry regcred --docker-server private-registry.io --docker-username registry-user --docker-password registry-password --docker-email register-user@org.com
```
{% endcode %}

* Then we specify the private image URL and the secret so kubelet on worker nodes can use this secret to pull your private image

### Lab (Review):

The most critical mistake I did is thinking that the `imagePullSecrets` is under `containers:` and its not correct, it must be under `spec:`

## Security Context

Containers aren't fully isolated like virtual machines, they share same kernel with host.

Containers are in their own namespaces and they can't see anything outside this namespace.

#### By default, docker runs processes as root user, if you want to change the user from root to a normal user:

```
docker run --user=1000 ubuntu sleep 60
```

Or you can specify the user ID within the docker image itself

{% hint style="info" %}
Docker limits the abilities of the root user within the container which is not the same as root user on a linux host machine
{% endhint %}

#### Docker limits some of these capabilities by default, although you can add or drop capabilities:

```
docker run --cap-add NET_BIND ubuntu # Add NET_BIND capability
docker run --cap-drop NET_BIND ubuntu # Remove NET_BIND capability
docker run --priviliged ubuntu # Include all capabilities
```

You can configure all of these at a pod-level in an object definition yaml within kubernetes using `securityContext`

#### At a pod level:

#### At a container level:

{% hint style="danger" %}
Capabilities are only supported at the container level and not at the pod level
{% endhint %}

### Lab:

There was a syntax issue and I faced a problem where I edited the pod and made sure I added the securityContext but whenever I created it, it was created without security context, so I had to delete it and do another pod with the requirements.

#### Useful commands that must be used:

```
kubectl delete pod ubuntu-sleeper --force
kubectl create pod ubuntu-sleeper.yaml
```

{% hint style="warning" %}
kubectl apply caused problems, specially when dealing with pods, so lets move to kubectl create if problems were faced with apply
{% endhint %}

Also, It's important to note the difference between pod level and container level, pod level is under `spec:` and above `containers:`, container level is under `containers:`

Container level `securityContext` overrides pod level `securityContext`

{% embed url="https://kubernetes.io/docs/tasks/configure-pod-container/security-context/" %}

## Network Policy

#### Let's say we have:

* Web Frontend Pod
* Backend Pod
* DB Pod

And we are required to create a network policy for the DB Pod to allow only Ingress traffic from the Backend pod at port 3306.

Now, DB pod only allow ingress traffic from the backend pod on port 3306 and blocks all others.

#### To create a network policy:

* Create a label and a selector in the DB pod
* Create the policy rule

{% hint style="danger" %}
In the policy type we defined Ingress type only, so egress is not affected and its by default allowing all egress traffic. If needed add egress rule too.
{% endhint %}

#### Also If you want to include same pods ingress rule in different namespaces, you can add a `namepsaceSelector:`

{% hint style="danger" %}
If we add a -namespaceSelector and make it under the `from:` it will be 2 rules not connected together (api-pod is allowed from anywhere, all pods in namespace prod are allowed), in the given example above it means that pod must be api-pod and in prod namespace
{% endhint %}

This will allow all pods named `api-pod` in the prod namespace to access the DB at port 3306, if we removed the `api-pod` then it will allow all pods within the prod namespace.

#### If you want to allow certain IP addresses traffic, you could specify an `ipBlock:`

#### Also, we could specify an agress rules as follows:

This will allow all traffic at port 80 originating from the DB pod to the backup server at the CIDR block addresses given.

### Lab:

#### I must change the `matchLabels: role:db` to the label in the pod that I want to write the policy for:

#### Here is my final policy:

#### And here is how the k8s docs helped:

{% embed url="https://kubernetes.io/docs/concepts/services-networking/network-policies/#default-policies" %}

## Kubectx & Kubens

### **Kubectx**

With this tool, you don't have to make use of lengthy “kubectl config” commands to switch between contexts. This tool is particularly useful to switch context between clusters in a multi-cluster environment.

**Installation:**

```
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectxsudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
```

**Syntax:**

To list all contexts:

> kubectx\\

To switch to a new context:

> kubectx \<context\_name>

To switch back to previous context:

> kubectx -

To see current context:

> kubectx -c

### **Kubens**

This tool allows users to switch between namespaces quickly with a simple command.

**Installation:**

{% code overflow="wrap" %}
```bash
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectxsudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```
{% endcode %}

**Syntax:**

To switch to a new namespace:

> kubens \<new\_namespace>

To switch back to previous namespace:

> kubens -

## Slides
