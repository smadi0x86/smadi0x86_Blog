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

```c
void delete(struct node *ptr, int position) {

    // If the current head node is to be deleted
    if (position == 1) {
        while (ptr->next != head) {
            ptr = ptr->next;
        }
        if (ptr == head) {
            free(head);
            head = NULL;
        } else {
            // Change the link
            ptr->next = head->next;
            // Deallocate the memory
            free(head);
            // Change head
            head = ptr->next;
        }
    }
    
    // If node from any other position is to be deleted
    else {
        // Traverse to the required node
        for (int i = 0; i < position - 2; i++) {
            ptr = ptr->next;
        }
        // Pointer for the node to be deleted
        struct node *toDelete;
        toDelete = ptr->next;
        // Change the link
        ptr->next = toDelete->next;
        // Deallocate the memory
        free(toDelete);
    }
}
```
