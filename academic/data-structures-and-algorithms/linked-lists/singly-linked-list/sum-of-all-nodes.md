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

# Sum Of All Nodes

```c
int sum(struct node *ptr) {

    // A variable to keep track of sum
    int sum = 0;
    
    // Iterate till ptr becomes NULL
    while (ptr != NULL) {
        sum += ptr->data;
        ptr = ptr->next;
    }
    return sum;
}
```

### Time & Space Complexity

#### Time Complexity: O(n)

#### Spoce Complexity: O(1)
