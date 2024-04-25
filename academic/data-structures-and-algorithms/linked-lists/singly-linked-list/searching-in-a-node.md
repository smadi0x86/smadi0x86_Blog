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

# Searching In A Node

```c
struct node * search(struct node *ptr, int key) {

    while (ptr != NULL) {
        if (ptr->data == key) {
            // Return address of node if found
            return ptr;
        }
        ptr = ptr -> next;
    }
    return NULL;
}
```

### Improving Using Move to Head Technique <a href="#improving-using-move-to-head-technique" id="improving-using-move-to-head-technique"></a>

```c
struct node * search(struct node *ptr, int key) {

    // A pointer which will follow ptr
    struct node *prev = NULL;
    
    while (ptr != NULL) {
        if (p -> data == key) {
            // Move the found node to the start 
            prev->next = ptr -> next;
            ptr->next = head;
            head = ptr;
            return ptr;
        }
        prev = ptr;
        ptr = ptr->next;
    }
    return NULL;
}
```
