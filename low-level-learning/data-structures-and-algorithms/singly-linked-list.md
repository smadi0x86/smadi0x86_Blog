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

# Singly Linked List

Every Linked List will have a `HEAD` which is the start of every linked list (A pointer to the rest of the list)

If this `HEAD` points to NULL, we know for a fact that this list is empty.

#### In a singly linked list, we can do the following:

* Add a NODE to a list
* Delete a NODE from a list
* Insert a NODE to a list

## Adding a NODE

All we have to do for adding a NODE to a list is to let the HEAD point to the new allocated NODE.

#### If we have our HEAD pointing to a pre-allocated NODE and we want to add a new NODE to the list, we have 2 ways:

* **Add node at the end:** We have to walk the entire list until we reach NULL which is O(n) and isn't efficient.
* **Insertion at front: We first allocate a new node, we point the new node to HEAD and then we point HEAD to our new node, this is O(1).**

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

### Implementation:

####
