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

# Reversing A Linked List

## Reversing by Changing Node Data & Using an Array <a href="#reversing-by-changing-node-data-and-using-an-array" id="reversing-by-changing-node-data-and-using-an-array"></a>

* Pass the pointer to head node and an array as parameters.
* Using a while loop, copy the data from each node to the array until the pointer becomes NULL.
* Set the pointer to head and decrement to index i by 1 to reach the last element of array.
* Overwrite the data in each node using a while loop and keep decrementing the index i until pointer becomes NULL.

```c
void reverse(struct node *ptr, int a[]) {
    
    // A variable to keep track of array index
    int i = 0;
    
    // Copy data from linked list to array
    while (ptr != NULL) {
        a[i] = ptr->data;
        i++;
        ptr = ptr->next;
    }
    
    // Set index to begin reverse procedure
    ptr = head;
    i--;
    
    // Copy data from array to linked list in reverse
    while (ptr != NULL) {
        ptr->data = a[i];
        i--;
        ptr = ptr->next;
    }
}
```

## Reversing using Sliding Pointers <a href="#reversing-using-sliding-pointers" id="reversing-using-sliding-pointers"></a>

* Take two additional pointers, prev and next, and initialize them with NULL.
* Copy the next address of each node to the next pointer and then set the next address of node to the prev pointer.
* Make current as next and prev as current in order to move forward.
* Do this procedure until current becomes NULL.
* Set the head node as prev pointer.

```c
void reverse(struct node *ptr) {

    // Consider ptr to the pointer to current node
    struct node *prev = NULL;
    struct node *next = NULL;

    while (ptr != NULL) {
        // Store the address of next node
        next = ptr->next;
        // Make the next of each node point to its previous
        ptr->next = prev;
        // Move forward
        prev = ptr;
        ptr = next;
    }
    // Setting the value of head node
    head = prev;   
}
```
