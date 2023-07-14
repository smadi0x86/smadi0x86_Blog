---
description: >-
  Tunneling happens among different protocols when you use a service by
  encapsulating it within another service.
cover: https://website.cdn.group-ib.com/wp-content/uploads/blog-2.png
coverY: 747
---

# Tunelling

## <mark style="color:red;">Forwarder.py</mark>

* _**Let's take a look at a program called "forwarder.py" which basically forward data from source to destination.**_

{% code fullWidth="false" %}
```
Pass source and destination IP and port:
```
{% endcode %}

<figure><img src="https://user-images.githubusercontent.com/75253629/230741661-d97236bb-7de4-4b1a-982d-512a3a49647c.png" alt=""><figcaption></figcaption></figure>

{% code fullWidth="false" %}
```
Then it will create a socket to listen on this IP and port, a server that is listening:
```
{% endcode %}

<figure><img src="https://user-images.githubusercontent.com/75253629/230741701-3a798003-86cc-4b0b-87e7-0ee3e5dcdce0.png" alt=""><figcaption></figcaption></figure>

{% code fullWidth="false" %}
```
When a client connects to it, it will establish a new TCP connection to target host and port.

And its an endless loop (while true).

When client send some data it will read it and send to target and vice versa.
```
{% endcode %}

<figure><img src="https://user-images.githubusercontent.com/75253629/230741748-4461b6ed-3349-4639-80d2-390a485af0a9.png" alt=""><figcaption></figcaption></figure>

{% code fullWidth="false" %}
```
To try this we need to connect to netcat using 1337 port for example.

Then use netcat to listen on port 1234.

Finally, we use the forwarder to forward packets from port 1337 to 1234 so they can communicate.
```
{% endcode %}

<figure><img src="https://user-images.githubusercontent.com/75253629/230741840-711794f2-e4eb-4e0d-9a1b-51424fb739b5.png" alt=""><figcaption></figcaption></figure>

{% code fullWidth="false" %}
```
We can also use this forwarder on a server on the internet.
We listen on all interfaces 0.0.0.0 on port 1337 and forward all traffic to ipinfo.io:80
```
{% endcode %}

&#x20;&#x20;

<figure><img src="https://user-images.githubusercontent.com/75253629/230741934-2ddd94a3-4c59-447b-bf41-49953b59a696.png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="https://user-images.githubusercontent.com/75253629/230741904-697bdef9-8030-472b-86a9-1b84adf2ddef.png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="https://user-images.githubusercontent.com/75253629/230741886-d281c37b-d3d6-437b-b4f8-c8b3246ec143.png" alt="" width="563"><figcaption></figcaption></figure>

```
Let's test it with curl:
```

```
curl -v http://forwarderIP:1337 -H "Host: ipinfo.io"
```

* _**v: display verbose output.**_
* _**-H: Custom HTTP header ( Not important for now)**_

&#x20;

<figure><img src="https://user-images.githubusercontent.com/75253629/230742287-424dbb8c-a561-4e2b-84ee-9596e261b18a.png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="https://user-images.githubusercontent.com/75253629/230742255-8dd7a66c-5632-4c63-bcf0-0c9fd4263348.png" alt="" width="563"><figcaption></figcaption></figure>

{% code fullWidth="false" %}
```
We just created a simple proxy or we can call it a tunnel for now,
To tunnel connections from 0.0.0.0:1337 to ipinfo.io.

But, this communication is not secure, we can use xor encryption in code to secure it.
```
{% endcode %}

{% hint style="info" %}
```
Now lets take a look at VPNs, a whole virtual private network.
VPNs forward the entire TCP packet, not just the data inside the packet.
```
{% endhint %}

* _**This is similar to the physical layer, but instead we use a VPN network which consists of layers again.**_

<figure><img src="https://user-images.githubusercontent.com/75253629/230742565-ace26c57-7596-413d-8bcc-11b113d822af.png" alt="" width="188"><figcaption></figcaption></figure>

* _**VPN client (VPN user) send complete packet ( wrapping the original TCP packet) to the VPN server.**_
* _**VPN server then unpacks the IP layer and TCP layer gives that to openvpn server for example.**_

<figure><img src="https://user-images.githubusercontent.com/75253629/230742703-d10df243-b171-407c-a84b-2e165a55f1a0.png" alt="" width="188"><figcaption></figcaption></figure>

* _**The openvpn server does its magic (decrypts data for example)**_
* _**Finally, it puts the data on the wire in its local network.**_

<figure><img src="https://user-images.githubusercontent.com/75253629/230742711-f0177c40-c4d0-499a-a8f9-69d039203e62.png" alt="" width="188"><figcaption></figcaption></figure>

{% code fullWidth="false" %}
```
That is what a tunnel is.

Any actual VPN takes complete network packets, forward them to remote computer,
then releases them into the remote network.
```
{% endcode %}

{% hint style="info" %}
{% code fullWidth="false" %}
```
How to create your own VPN software to get the TCP/IP packet and then send it to another server?

All VPNs are based on a feature called TUN/TAP (virtual/fake emulated network card).
Your computer has an interface usually called eth0, and all services and programs identify it as a network card.
It is configured to handle certain traffic, the OS uses interface to reach you.

TUN/TAP is the same, but its non existence, you can tell your OS to route all packets over TUN/TAP.
```
{% endcode %}
{% endhint %}

{% embed url="https://backreference.org/2010/03/26/tuntap-interface-tutorial/index.html" %}
