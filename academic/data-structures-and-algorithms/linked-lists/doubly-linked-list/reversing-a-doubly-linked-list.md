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

# Reversing A Doubly Linked List

```c
void reverse(struct node *ptr) {

    // Temporary pointer for swapping
    struct node *temp;
    
    while (ptr != NULL) {
        // Swapping the links
        temp = ptr->next;
        ptr->next = ptr->prev;
        ptr->prev = temp;
        // Moving forward
        ptr = ptr->prev;
        // Condition to set head node
        if (ptr != NULL && ptr->next == NULL) {
            head = ptr;
        }
    }
}
```
