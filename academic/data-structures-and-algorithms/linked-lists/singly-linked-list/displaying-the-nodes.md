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

#### Iterative Method: <a href="#iterative-method" id="iterative-method"></a>

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

#### Recursive Method: <a href="#recursive-method" id="recursive-method"></a>

```c
void display(struct node *ptr) {

    if (ptr != NULL) {
        printf("%d\t", ptr->data);
        display(ptr->next);
    }
}
```

#### Recursive & Reversed: <a href="#recursive-and-reversed" id="recursive-and-reversed"></a>

```c
void display(struct node *ptr) {

    if (ptr != NULL) {
        display(ptr -> next);
        printf("%d\t", ptr->data);
    }
}
```

#### Time and Space Complexity: <a href="#time-and-space-complexity" id="time-and-space-complexity"></a>

{% tabs %}
{% tab title="Iterative" %}
Time Complexity: **O(N)**&#x20;

**No extra space**
{% endtab %}

{% tab title="Recursive" %}
Time complexity : **O(N)**&#x20;

**Internal stack of size n+1 is used.**
{% endtab %}
{% endtabs %}
