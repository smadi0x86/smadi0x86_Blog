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

# Displaying The Nodes

```c
void display(struct node *ptr) {
    // Checking if list is empty
    if (ptr == NULL) {
        printf("The list is empty\n");
    }
    // Iterate till the end otherwise
    else {
        while (ptr != NULL) {
            printf("%d\t", ptr->data);
            ptr = ptr->next;
        }
    }
}
```

### Time & Space Complexity <a href="#time-and-space-complexity" id="time-and-space-complexity"></a>

Time Complexity: **O(n)**&#x20;

**No extra space**
