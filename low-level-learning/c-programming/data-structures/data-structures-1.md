---
cover: https://cdn.pixabay.com/photo/2016/07/27/10/44/fireworks-1544999_1280.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Structs

## <mark style="color:red;">**Definition & Declaration**</mark>

Structures are user-defined data types in C that allow grouping different types of data under a single name.

```c
struct date {
    int month;
    int day;
    int year;
};
```

#### <mark style="color:purple;">To use the structure:</mark>

```c
struct date today;  // variable of type `date`
today.month = 5;
```

## <mark style="color:red;">**Direct Initialization**</mark>

#### <mark style="color:purple;">Directly upon declaration:</mark>

```c
struct date today = {1, 5, 2019};
```

#### <mark style="color:purple;">Fewer values than structure members:</mark>

```c
struct date today = {1, 5};  // No value for `year`
```

#### <mark style="color:purple;">Using member names for clarity:</mark>

```c
struct date date1 = {.month = 12, .day = 23};
```

## <mark style="color:red;">**Compound Literals (C11 only)**</mark>

They provide a way to assign values to structures in a single statement.

```c
today = (struct date){9, 12, 2016};
```

## <mark style="color:red;">**Structure Arrays**</mark>

#### <mark style="color:purple;">Declaration of an array of structures:</mark>

```c
struct date myDates[10];
```

#### <mark style="color:purple;">Initialization:</mark>

```c
myDates[1].month = 6;
```

OR

```c
struct date myDates[5] = {12, 1, 1985, 12, 5, 1984, 11, 12, 1998};
```

## <mark style="color:red;">**Structures Containing Arrays**</mark>

#### <mark style="color:purple;">Structures can contain arrays as members:</mark>

```c
struct month {
    int numberOfDays;
    char name[3];
};
```

This can represent a month with its number of days and a 3-character name.

## <mark style="color:red;">**Nested Structures**</mark>

#### <mark style="color:purple;">Allows for structures to contain other structures:</mark>

```c
struct datetime {
    struct date sdate;
    struct time stime;
};
```

## <mark style="color:red;">**Pointers to Structures**</mark>

#### <mark style="color:purple;">Declaring a pointer to a struct:</mark>

```c
struct date *datePtr;
```

#### <mark style="color:purple;">Setting and accessing data:</mark>

```c
datePtr = &today;
datePtr->month = 12;  // Equivalent to (*datePtr).month = 12;
```

## <mark style="color:red;">**Structures and Functions**</mark>

#### <mark style="color:purple;">Structures can be both arguments to functions and return types:</mark>

```c
bool siblings(struct family member1, struct family member2);
struct Date my_fun(void);
```

## <mark style="color:red;">**Structures Containing Pointers**</mark>

#### <mark style="color:purple;">Structures can also contain pointers:</mark>

```c
struct intPtrs {
    int *p1;
    int *p2;
};
```
