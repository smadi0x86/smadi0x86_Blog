---
cover: https://cdn.pixabay.com/photo/2022/06/12/22/48/gradient-7258997_960_720.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Linked Lists

## <mark style="color:red;">Introduction</mark> <a href="#introduction" id="introduction"></a>

A linked list is a data structure that stores a collection of objects in a sequential manner.&#x20;

* Each item in list is stored in a **node**.&#x20;
* Each node consists of a single piece of data (it is permissible for the data to be an aggregate type) along with at least one pointer to another node, typically the next node within the list.&#x20;

The linked list object itself must contain a pointer to a node within the list that will allow access to every other node by following the pointers in the node. Usually this is a pointer to the first node in the linked list.

#### <mark style="color:purple;">If you wanted to store an array of 5 numbers in an array this is what the array would look like:</mark>

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LIA-o6Gkqe2woIh4ULd%252F-LIA-foN63KefOSBsvyH%252Farray.png%3Falt%3Dmedia%26token%3Dd1243192-7bb5-4ecf-8240-9b6850989516&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=30d335112b116ce287e710f3129c1a70474dc892eef210a9c18d49dcd1b83ad3" alt=""><figcaption><p>Array of 5 Float Numbers</p></figcaption></figure>

#### <mark style="color:purple;">A simple linked list that stores the same 5 numbers in the same sequence would look like this:</mark>

<figure><img src="https://catherine-leung.gitbook.io/~gitbook/image?url=https%3A%2F%2F3939692842-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LFrWzEqLSRHjU6HG9dw%252F-LLGtWKsOQl6ie6AJXqv%252F-LLGtcXDpaYldK2ZKm1O%252Fbasicsl.png%3Falt%3Dmedia%26token%3D9c6f4f58-b526-4429-b08a-ba6a62a43303&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=1732debc98422e0839536d9b63c097aef8f5efa854c02e43cb9f2c0d3948df46" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The above is a **singly linked list**. It is called this because there is a single pointer out of each node to another node.
{% endhint %}

{% hint style="warning" %}
Note that while the above diagram only has a single pointer out of the linked list object itself, it is not part of the definition of a singly linked list. The single link only refers to the link from one node to another.
{% endhint %}

## <mark style="color:red;">Common Operations</mark> <a href="#typical-operations" id="typical-operations"></a>

#### <mark style="color:purple;">To look at how to implement a linked list, the operations a linked list can do should be considered. A linked list can typically support some subset of the following operations:</mark>

* **push\_front** - add an item to the front of the linked list
* **push\_back** - add an item to the back of the linked list
* **pop\_front** - remove the frontmost item from the linked list
* **pop\_back** - remove the backmost item from the linked list
* **insert** - given a point within the list insert an item just before that point
* **erase** - remove a node at a specific point within the list
* **erase(a,b)** - erases all nodes between a and b
* **traversals** - some operation that applies to every node in the list

{% hint style="info" %}
To support the above operations, a linked list typically will store more information than the basic list. We will detail these in the implementation section.
{% endhint %}
