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

# Arrays

Arrays allow grouping values under a single name, the data items in an array are referred to as elements.

## <mark style="color:red;">Initializing an array (lists)</mark>

**float sample\_data\[500] = {1,2,3,4,5}; →** The capacity is 500 values but we declared only 5 so there will be 495 empty values available and these numbers (1 to 5) are assigned to the first three elements of the array.

## <mark style="color:red;">Designated initializers</mark>

c99 added a feature called designated initiazlizers allowing us to pick and choose which elements are initialized.

We can assign array elements values in any order by enclosing an element number in a pair of brackets.

```c
float simple_data[500] = { [2] = 500.5, [1] = 300.0, [0] = 1000.0 };
```

So here element number 2 of array simple\_data is 500.5 and so on.

```c
#define months 12
int days[months] = {31,28,[4]=31,30,31,[1]=29};
```

#### <mark style="color:purple;">To initialize a range in array:</mark>

```c
int a[] = {start..end};
int a[] = { [0..9] = 1, [10..100] = 23 };
```

## <mark style="color:red;">Two-dimensional arrays</mark>

The c language allows arrays of any dimensions to be defined two dimension arrays are the most common one and the most natural application of this is the case of a matrix.

```c
int matrix[4][5]; → 4 rows and 5 columns for a total elements of 20
int matrix[4][3] = {[0][0] = 1, [1][1] = 5 ,[2][2]= 9};
```

#### <mark style="color:purple;">Example:</mark>

```c
 int numbers[3][4]= {
      {10,20,30,40}, // values for first row
      {15,25,35,45}, // values for second row
      {47,48,49,50} // values for third row
};
```

## <mark style="color:red;">Three-dimensional arrays</mark>

For storing x, y, z values.

```c
 int matrix[4][5][3]; 
 int matrix[4][2][3] = {[0][0][0] = 1, [1][1][1] = 5 ,[2][2][2]= 9};
```

#### <mark style="color:purple;">Example:</mark>

```c
int numbers[2][3][4]= {
        {   // first block of 3 rows
            
                {10, 20, 30, 40}, // values for first row
                {15, 25, 35, 45}, // values for second row
                {47, 48, 49, 50} // values for third row
        },
        
        {   // second block of 3 rows
            
                {10, 20, 30, 40}, // values for first row
                {15, 25, 35, 45}, // values for second row
                {47, 48, 49, 50} // values for third row
            
        }
```

## <mark style="color:red;">Processing elements in a N dimensional array</mark>

```c
int numbers[2][3][4];
int sum = 0;

    for(int i=0; i<2; ++i) 
    {
        for(int j = 0 ; j < 3 ; ++j)
        {
            for(int k = 0 ;k < 4 ; ++k)
            {
                sum += numbers[i][j][k];
            }
        }
    }
```

## <mark style="color:red;">Variable length arrays (VLA)</mark>

It allows you to specify the size of an array with a variable when creating an array.&#x20;

{% hint style="info" %}
Introduced in c99.
{% endhint %}

A VLA keeps the same size after creation.

```c
 float letters[n]; →  n is a variable which specifies the length of letters array.
```

## <mark style="color:red;">Flexible array members</mark>

A feature introduced in the c99 standard of the c language.&#x20;

When using a structure we can declare an array without a dimension and whose size is flexible in nature.

A flexible array members size is variable (can be changed at runtime).

```c
 struct s {
   int arraySize;
   int array[];
 }; 
```

#### A flexible array member is declared by specifying empty square brackets \[].

{% hint style="info" %}
A flexible array can be declared only as the last member if a struct.

Each struct can contain at most one flexible array member.

A flexible array cannot be the only member of a struct.

Any struct containing a flexible array member cannot be a member of another struct.

A struct with a flexible array member cannot be statically initialized, it must be allocated dynamically.

You cannot fix the size of the flexible array member at compile time.
{% endhint %}
