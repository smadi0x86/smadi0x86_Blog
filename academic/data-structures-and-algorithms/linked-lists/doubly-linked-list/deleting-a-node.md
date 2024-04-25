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
void delete(struct node *ptr, int pos) {

    // If first node is to be deleted
    if (pos == 1) {
        head = head->next;
        if (head) {
            head->prev = NULL;
        }
        free(ptr);
    }

    // Deleting any other node
    else {
        // Reach the node to be deleted
        for (int i = 0; i < pos - 1; i++) {
            ptr = ptr->next;
        }
        // Modify the links
        ptr->prev->next = ptr->next;
        if (ptr->next) {
            ptr->next->prev = ptr->prev;
        }
        // Deallocate the memory
        free(ptr);
    }
}
```
