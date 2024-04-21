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

# Stack & Queues

## <mark style="color:red;">Introduction</mark> <a href="#introduction" id="introduction"></a>

A _**stack**_ is a kind of list where items are always added to the front and removed from the front. Thus, a stack is a _**Last In, First Out**_ (LIFO) structure.&#x20;

A stack can be thought of a structure that resembles a stack of trays. Each time a new piece of data is added to the stack, it is placed on the top. Each time a piece of data is removed it also must be removed from the top. Typically only the top item is visible. You cannot remove something from the middle.

_**Queues**_ like stacks are a special kind of list. In the case of a queue, items are added to the back and removed from the front. In other words a queue is a First In First Out (FIFO) structure. A queue is essentially a line up.

{% hint style="info" %}
The idea of _**front,**_ _**back**_, and _**top**_ in stacks and queues have absolutely nothing to do with their positions within the data structures used to implement them.&#x20;

It is up to the implementer to make that decision on where to put the frontmost, topmost and backmost items and maintain order.
{% endhint %}

## <mark style="color:red;">Operations</mark> <a href="#operations" id="operations"></a>

#### Typically stacks and queues have the ability to do the following:

* Add an item
* Remove an item
* Access the "next" item to be removed

#### There are various names that we attribute to these operations for these:

| <mark style="color:blue;">**Operation**</mark> | <mark style="color:blue;">**Stack**</mark> | <mark style="color:blue;">**Queue**</mark> |
| ---------------------------------------------- | ------------------------------------------ | ------------------------------------------ |
| Add an item                                    | push                                       | enqueue                                    |
| Remove an item                                 | pop                                        | dequeue                                    |
| Access the "next" item to be removed           | top                                        | front                                      |

The most important thing to remember about stacks and queues is that its all about the ordering. This absolutely must be tracked and maintained. When you add an item it must be positioned as the newest item. When you remove an item the one you remove is always the newest item for stacks or the oldest item for queues.&#x20;

You can't put something into the middle of a stack or queue. When you remove there is only one item that you can remove. Access is typically only granted to the item that is at the top/front of the stack/queue.

{% hint style="warning" %}
Stacks and Queues are NOT for general storage. They are used to track ordering. Any other features other than the 3 above must be secondary.
{% endhint %}

## <mark style="color:red;">Applications</mark> <a href="#applications" id="applications"></a>

Applications of stacks and queues typically involve tracking the ordering of a set of values. The question is whether the application is interested in the data in the same order as it was received or reverse.&#x20;

{% hint style="info" %}
Note that this is not as straight forward as it may seem. We are not talking about encountering all the data in one go. It will typically involve continuous adding and removing of data to the Stack or Queue.
{% endhint %}

**Stack Applications:**

1. **Thread Execution**: Each thread in an operating system has its own stack that holds the thread's local variables, function parameters, return addresses, and the call history. This isolation helps in managing thread-specific execution contexts.
2. **System Calls**: When a system call is made, the OS uses a stack to store the call's parameters and state, ensuring that after the system call is handled, the user process can continue where it left off.

**Queue Applications:**

1. **Load Balancing**: In cloud environments, queues are used to distribute workloads evenly across computing resources. Tasks are queued and then processed by available servers based on demand, enhancing scalability and efficiency.
2. **Service Orchestration**: In microservices architectures, API gateways utilize queues to orchestrate calls to various services. This involves queuing requests that involve multiple service interactions and managing the order and completion of these service calls to deliver a final response.
