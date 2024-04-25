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

# Deleting A Node

## Deleting the Head Node <a href="#deleting-the-head-node" id="deleting-the-head-node"></a>

```c
void deleteFirst(struct node *ptr) {

    first = ptr->next;
    free(ptr);
}
```

## Deleting a Node from Any Index <a href="#deleting-a-node-from-any-index" id="deleting-a-node-from-any-index"></a>

```c
void delete(struct node *ptr, int index) {

    // If node to be deleted is the head node
    if (index == 1) {
        head = ptr->next;
        free(ptr);
    }
    
    // Deleting other nodes
    else {
        for (int i = 0; i < index - 2; i++) {
            ptr = ptr->next;
        }
        // Find the node to be deleted
        struct node *toDelete = ptr->next;
        // Replace the link
        ptr->next = toDelete->next;
        // Deallocate the memory
        free(toDelete);
    }
}
```
