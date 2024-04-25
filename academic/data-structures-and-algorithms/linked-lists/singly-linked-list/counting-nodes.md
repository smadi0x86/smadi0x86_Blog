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

# Counting Nodes

```c
int count(struct node *ptr) {

    // A variable to keep track of count
    int count = 0;

    // Iterate till ptr becomes NULL
    while (ptr != NULL) {
        count++;
        ptr = ptr->next;
    }
    return count;
}
```

### Time & Space Complexity

#### Time Complexity: O(n)

#### Spoce Complexity: O(1)
