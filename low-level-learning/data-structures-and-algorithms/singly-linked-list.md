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

A linked list a data structure, used for fast and memory efficient insertions and deletions.&#x20;

Unlike arrays, which are single objects, linked lists contain multiple `node` objects that are linked together via pointers.

Linked lists can either be **singly** or **doubly** linked. We'll focus on singly linked lists for now.

## **Singly Linked List**

If linked lists are multiple objects linked together, ideally we need a couple different objects.

1. A LinkedList object, which holds all of the objects in the list.
2. A Node object, which contains the `data` for the element, as well as a **pointer** to the next Node in the list.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### **Wait, why not use arrays instead?**

In lower level languages, arrays are allocated in **blocks.** Therefore, arrays are static in size and can only hold a specific data type.&#x20;

Linked lists store data in the **heap,** meaning that the data can be stored in an unorganized manner. Since each node points to the next one, it's still possible to maintain the list structure.

### Getting Started <a href="#getting-started" id="getting-started"></a>

The best way to understand how linked lists work is to make one! Let's do so in C.&#x20;



### Iterating Over Linked Lists <a href="#iterating-over-linked-lists" id="iterating-over-linked-lists"></a>

Use while loops to iterate over a linked lists.&#x20;

While loops are better than for loops because Linked Lists don't have a length property.&#x20;

Instead of iterating `for` a certain number of times, we simply iterate `while` our `current` pointer is not `NULL`.

Create a a local variable called `current` that starts off pointing to the list's root node.&#x20;

#### Inside the loop you can do two things:

1. Access the value of the current node at `current->nodeData`
2. Point the current variable to the next item with `current->nextNode`

```c
/*
 * Delete a NODE from the list
 */
void deleteNode(int nodeData)
{
    Node *current = HEAD;
    Node *previous = HEAD;

    while (current != NULL)
    {
        if (current->nodeData == nodeData)
        {
            if (current == HEAD)
            {
                HEAD = current->nextNode;
            }
            else
            {
                previous->nextNode = current->nextNode;
            }
            free(current);
            return;
        }
        previous = current;
        current = current->nextNode;
    }
}
```
