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

# Inserting A Node

```c
void insert(struct node *ptr, int data, int pos) {

    // Creating a new node
    struct node *newNode;
    newNode = (struct node *)malloc(sizeof(struct node));
    newNode->data = data;

    // If node is to be inserted at first position
    if (pos == 1) {
        if (head == NULL) {
            newNode->prev = NULL;
            newNode->next = NULL;
            head = newNode;
        } else {
            newNode->prev = NULL;
            newNode->next = head;
            head = newNode;
        }
    }

    // If node is to be inserted in any other position
    else {
        for (int i = 0; i < pos - 2; i++) {
            ptr = ptr->next;
        }
        newNode->next = ptr->next;
        newNode->prev = ptr;
        // Check if next node is available
        if (ptr->next) {
            ptr->next->prev = newNode;
        }
        ptr->next = newNode;
    }
```
