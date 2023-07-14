---
description: Notes for my university's networking course.
cover: >-
  https://img.freepik.com/premium-photo/abstract-wave-grid-neon-color-mesh-light-effect-d-illustration_357568-614.jpg?w=1800
coverY: 0
---

# Packet Tracer

{% file src="../.gitbook/assets/Networking_final_PT (1).pkt" %}

{% file src="../.gitbook/assets/Formative.pkt" %}

## <mark style="color:red;">Steps of configurations</mark>

1. Design your network topology such as star, ring, bus, point to point etc...
2. Assign static IP addresses to servers and devices if you are asked to.
3. Configure routing on routers using static routing or dynamic routing (OSPF, RIPv2, BGP etc..).
4. Configure DNS for translating IP to domain names.
5. Configure HTTPS for web browsing server (website).
6. Configure DHCP for assigning dynamic IPs to network pools.
7. Configure Email (SMTP & POP3) for allowing email sending between networks.
8. Configure FTP for allowing data exchange such as files and images between network devices.

## <mark style="color:red;">Network Topology</mark>

When planning a network, it is crucial to consider a topology that is suitable for your needs while providing Cost-efficiency, Performance, Security, Scalability and Sustainability.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:red;">Routing</mark>

### <mark style="color:yellow;">Static</mark>

{% embed url="https://yaser-rahmati.gitbook.io/gns3/lab-3-configure-static-route-in-gns3" %}

### <mark style="color:yellow;">RIPv2</mark>

{% embed url="https://yaser-rahmati.gitbook.io/rahmati-academy/Tutorials-Library/cisco-adademy/ccna/lab-configuring-basic-ripv2" %}

### <mark style="color:yellow;">OSPF</mark>

1. R1 > enable
2. R1 > configure terminal
3. R1 > router ospf 1
4. R1 > network \<serial/gig> \<wildcard which is 0.0.0.255 - last octet of sub mask> area \<same number between routers, for example: 0>
5. CTRL + C
6. show ip protocols

{% hint style="info" %}
Now go to other router connected to the router you did above.
{% endhint %}

{% hint style="info" %}
passive-interface \<gig> to deny the hello messages sent by gig, do it on routers that are directly connected to gig interfaces.
{% endhint %}

1. R2 > enable
2. R2 > configure terminal
3. R2 > router ospf 1
4. R2 > network \<serial IP connected to the previous router> \<wildcard> \<area same as previous router>

{% hint style="success" %}
Now you should get a message saying loading to full which means that it worked.
{% endhint %}

<figure><img src="../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Its recommended to do routing for every 2 networks at a time, so it will be easier.
{% endhint %}

#### Give IPs on Istanbul router to GIG and serial then go to the router which is connected to Istanbul above and give it the serial IP its using to connect to Istanbul.&#x20;

#### The up serial connecting to Istanbul for example will be 100.0.0.9, the other serial of Istanbul connected to up router will be 100.0.0.10.

{% hint style="info" %}
After you give IPs to GIG and Serial, do the ospf above.
{% endhint %}

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:red;">DHCP</mark>

{% hint style="info" %}
#### Any PC that wants an IP is a client of the DHCP server
{% endhint %}

### <mark style="color:yellow;">The process of DHCP IP assigning</mark>

* **Discovery:** A device that is seeking for a DHCP server will broadcast a message to all interfaces.&#x20;
* **Offer:** After receiving the discovery message, the DHCP server sends an IP address as a unicast offer message to the PC.
* **Request:** When the PC accepts the IP address offered to it, it will request the IP address from the DHCP server.&#x20;
* **Acknowledgement:** When the DHCP gets a request from a PC, it first checks its table of IP addresses and mac addresses. If no other PCs are using the same IP address, it sends an acknowledgement message, indicating that it has accepted the request and it gives the PC the IP address.

{% hint style="danger" %}
**APIPA is being used:** When the DHCP fails to give an IP address to a device, the PC automatically contacts other PCs and they agree on certain IP addresses to be used by them which mainly start with 169 and uses subnet mask of 255.255.0.0
{% endhint %}

### <mark style="color:yellow;">Configure DHCP Server</mark>

{% hint style="info" %}
Always assign a static IP address to servers
{% endhint %}

1. Select the server "Server-PT" in packet tracer and name it DHCP.

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

2. Double click on server > Desktop > Configuration > Assign static IP

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

3. Exit the configuration > Services > DHCP > Turn service on

<figure><img src="../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

4. Change the Start IP Address and subnet mask to 0's for the default first pool and save it, we did this because it may cause problems in the distribution of IPs.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

4. Check how many networks you have and add pools based on it by giving every pool the first - last valid hosts excluding gateway (its added by itself in pool) and any services that require a static IP of each network and name every pool with the same network name to avoid confusion and know every pool is assigning IPs to which network then press add.

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

5. Go to every network that you added a pool to and choose the devices under it that you want to assign a dynamic IP to, on every device go to Desktop > Configuration > Press on DHCP then it should give you a DHCP request successful.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
If you want to assign dynamic IPs to a network outside the DHCP server network you must do static/dynamic routing so the DHCP server and PC can do the DORA process to reach each other.
{% endhint %}

### <mark style="color:yellow;">When you assign a DHCP IP to a device outside the DHCP server network for the first time, you will get this error message</mark>

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

This is happening because the first process of the DORA is discovery which is a broadcast message sent by the PC to look for a DHCP server, but the router will by default delete any broadcast messages sent to it to avoid high traffic in networks.

### <mark style="color:yellow;">Fixing this error using relay agent</mark>

We must define a relay agent command written on the router, to tell the router not to delete any broadcast messages that is intended to look for a DHCP server and just forward it as unicast to the DHCP server.

1. Go to the interface that is connected from the router to switch of the other network (GIG).
2. Press on CLI of the router.
3. enable > configure terminal > interface \<gig ...> > ip helper-address \<address of dhcp>
4. exit > exit > show running-config

<figure><img src="../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (80).png" alt="" width="541"><figcaption></figcaption></figure>

## <mark style="color:red;">Loopback</mark>

The loopback interface can be used to test routing and forwarding configurations within a device, allowing network engineers to verify the behavior of packets within the device itself (In this example we want to see the behavior of the default route to the ISP).

#### Go the the router you were asked to do a loopback interface on.

1. Enter router CLI
2. enable > configure terminal > interface loopback 0 > Ip address \<loopback Ip> \<loopback sub mask>
3. exit > exit
4. show Ip interfaces brief

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

#### After you did loopback you must:

* enable > configure terminal > router \<routing protocol ex: router ospf 1> > Ip route 0.0.0.0 0.0.0.0 loopback100: To create an ISP (default route).

{% hint style="info" %}
0s IP address and subnet mask is a way of saying catch all meaning it applies to all possible IP addresses which are not in local network and sent to the default route. It ensures that if a router doesn't have a specific route for a particular destination, it will use the default route as a last resort to forward the traffic.
{% endhint %}

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

* default-information originate: To tell all routers that I have an ISP to come to it if no Ip address is found in the local network and it points at a next hop typically an ISP or another router.

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

exit > exit > show Ip route: you must see a S\* on the loopback that means it worked.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
S\* is indicating that the default route is static and not affected by OSPF.
{% endhint %}

Also, do an enable > show Ip route on all routers to check if they have O\*E2 on the 0.0.0.0 route, this indicates that all routers are correctly configured and can see the default route which points at the ISP.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
O\*E2 ( OSPF External Type 2) It refers to a specific type of route that is learned through the OSPF routing protocol which in this case is the default route 0.0.0.0/0.
{% endhint %}

## <mark style="color:red;">Router password</mark>

Go to the router you want to assign a password to.

1. Enter router CLI
2. enable > configure terminal > line console 0 > password \<your password> > login
3. exit > exit > exit

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:red;">Services</mark>

### <mark style="color:yellow;">SMTP</mark>

<figure><img src="../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### <mark style="color:yellow;">FTP</mark>

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### <mark style="color:yellow;">HTTP & DNS</mark>

<figure><img src="../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:red;">Access Point & Laptop</mark>

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>
