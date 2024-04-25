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
void insert(struct node *ptr, int data, int postition) {

    // Creating a new node and assigning data
    struct node *newNode;
    newNode = (struct node *)malloc(sizeof(struct node));
    newNode->data = data; 

    // If node is to be inserted before head
    if (position == 1) {
        // Checking if list is empty
        if (head == NULL) {
            head = newNode;
            head->next = head;
        } 
        // If a node already exists
        else {
            // Traverse to end of list
            while (ptr->next != head) {
                ptr = ptr->next;
            }
            ptr->next = newNode;
            newNode->next = head;
        }        
    }
    
    // If node is to be inserted in any other index
    else {
        for (int i = 0; i < postition - 2; i++) {
            ptr = ptr->next;
        }
        newNode->next = ptr->next;
        ptr->next = newNode;
    }
}
```
