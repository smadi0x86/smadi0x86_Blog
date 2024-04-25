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

## Insert at Beginning <a href="#insert-at-beginning" id="insert-at-beginning"></a>

```c
void insertAtBeginning(struct node *ptr, int data) {

    // Creating a new node
    struct node *newNode;
    newNode = (struct node *)malloc(sizeof(struct node));
    newNode->data = data;
    
    // Make the new node as first node
    newNode->next = head;
    head = newNode;

}
```

## Insert at N-th Position <a href="#insert-at-n-th-position" id="insert-at-n-th-position"></a>

```c
void insertAtNthPos(struct node *ptr, int pos, int data) {

    // If position is invalid
    if (pos < 0 || pos > count(head)) {
        printf("Inavlid position\n");
        return;
    }
    
    else {
    
        // Creating a new node
        struct node *newNode;
        newNode = (struct node *)malloc(sizeof(struct node));
        newNode->data = data;
    
        // For valid position, traverse to the position
        for (int i = 0; i < pos - 2; i++) {
            ptr = ptr->next;
        }
        
        // Setting up links
        newNode->next = ptr->next;
        ptr->next = newNode;
    }
}
```

{% hint style="info" %}
Indexing for the linked list is considered to start from 1 in the above code where the index of the first node is 1.

If index is considered to start from 0, change condition to **pos - 1** instead of **pos - 2** in the for loop.
{% endhint %}

## Insert at End in O(1) Time <a href="#insert-at-end-in-o-1-time" id="insert-at-end-in-o-1-time"></a>

```c
void insertAtEnd(struct node *ptr, int data) {

    // Creating a new node
    struct node *newNode;
    newNode = (struct node *)malloc(sizeof(struct node));
    newNode->data = data;
    newNode->next = NULL;

    // If there are no nodes present
    if (head == NULL) {
        first = newNode;
        last = newNode;
    }
    
    // If some nodes are already present
    else {
        last->next = newNode;
        last = newNode;
    }
}
```

{% hint style="info" %}
In the above function, an **additional pointer named last** is taken which will always point to the last node of the linked list. Through this, we can insert a new node to the end of list in constant time.
{% endhint %}

## Insert at End in O(n) Time <a href="#insert-at-end-in-o-n-time" id="insert-at-end-in-o-n-time"></a>

```c
void insertAtEnd(struct node *ptr, int data) {

    // Creating a new node
    struct node *newNode;
    newNode = (struct node *)malloc(sizeof(struct node));
    newNode->data = data;
    newNode->next = NULL;

    // Traverse to last node
    while (ptr -> next != NULL) {
        ptr = ptr->next;
    }
    ptr->next = newNode;
}
```
