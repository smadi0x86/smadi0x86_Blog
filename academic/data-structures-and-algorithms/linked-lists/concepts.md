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

# Concepts

## <mark style="color:red;">Nodes</mark> <a href="#nodes" id="nodes"></a>

The basic unit of storage for a linked list is a _**node**_. A node stores data and one or more pointers to other nodes.&#x20;

At its most basic, a linked list node consists of data plus a pointer to the next node in the linked list. It is possible for a node to also store a pointer to the previous node in the linked list.

A linked lists where each node contains a single pointer to the next node is called a _**singly linked list**_.

A linked list where each node contains two pointers one to the next node and one to the previous node is called a _**doubly linked list.**_

The idea of storing data in a node is not unique to linked lists. Other data structures such as trees also store data in nodes.

## <mark style="color:red;">Iterators</mark> <a href="#iterators" id="iterators"></a>

Iterators allows you to traverse a container class without actually knowing what the underlying container actually is or how that container may be implemented. You can observe their usage in container classes within the C++ Standard Library such as \<vector> and \<list>. A linked list is a container class. Thus, our implementation of a linked list should also include the concept iterators. Iterators also allow us to traverse the list in various ways (backwards and forwards for example). The idea is simply to separate the thing that allows us to go through a list with the underlying data structure

#### <mark style="color:purple;">Iterators should support the following functionalities at minimum. Note that these are just general ideas. They are not language specific:</mark>

* **first** - set iterator to refer to the first item in a container
* **next** - sets iterator to the next item in the container
* **isDone** - returns true if iterator is not referring to anything
* **currentItem** - returns the current piece of data

When thinking about iterators, the important concept is that an iterator lets us go through a container one data item at a time. It provides a uniform method to do this as well as access our data.&#x20;

While you may view it as being similar to a node pointer, it is not the same. To the user of the list, there is no such thing as a node. They only have iterators. This is really important to remember.

{% hint style="danger" %}
An iterator is NOT a Node pointer. An iterator essentially hides the fact that there are even nodes within our linked list. It simply provides access to a set of data stored within a container in a uniform manner.
{% endhint %}
