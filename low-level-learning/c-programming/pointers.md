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

# Pointers

## <mark style="color:red;">Indirection</mark>

Is the ability to reference something using a name, reference or container instead of the value itself.

The most common form of indirection is the act of manipulating a value through its memory address.

A pointer provides an indirect means of accessing the value of a particular data item a variable whose value is a memory address.

C provides a remarkably useful type of variable called a pointer, a variable that stores a memory address.&#x20;

Its value is the address of another location in memory that can contain a value.

<figure><img src="../../.gitbook/assets/image (29).png" alt="" width="563"><figcaption></figcaption></figure>

Pointers allow functions to modify data passed to them as variables.&#x20;

Pass by reference passing arguments to function in a way they can be changed by function.&#x20;

Can also be used to optimize program performance and speed by using less memory.

## <mark style="color:red;">Declaring pointers</mark>

```c
int number = 2; int *numberPointer = &number;
```

"numberPointer" now contains the address where variable "number" is stored.

Pointers must have the same variable type of the address values that contain the data for example char pointer, float pointer etc...

%p ->Represents the format specified for a pointer.

**int \*pointer = NULL;** → is the equivalent of zero for a pointer, it doesn't point to any location in the memory.

## <mark style="color:red;">Address of operator</mark>

The pointer simply points to the address of the variable and can act as an operator for using that variable.

```c
int number = 00; 
int *pnumber = &number;
```

## <mark style="color:red;">De-referencing pointer to value</mark>

**printf("%p", pnumber);** → prints the pointer value printf("%d",\*pnumber); → prints the value that its pointed to (number).

## <mark style="color:red;">**D**</mark><mark style="color:red;">isplaying an address without a pointer</mark>

**printf("number address: %p", \&number);**

or

**printf("number address: %p", (void**_**) \&number);** → (void_) cast is to prevent possible warning from the compiler.

#### <mark style="color:purple;">Example:</mark>

```c
int number = 0; // a variable of type int
int *pnumber = NULL; // a pointer that can point to type int

number = 10;

printf("number address: %p\n", &number); // output the address
printf("number value: %d\n", number);  //output the value

pnumber = &number;  //store the address of number in pnumber

printf("pnumbers address: %p\n", &pnumber);  //output pnumber address
printf("pnumber size: %d bytes\n", sizeof(pnumber)); //output size of pnumber
printf("pnumber value: %p\n", pnumber); // output the value of pnumber
printf("value pointed to: %d\n", *pnumber); // value at the address
```

## <mark style="color:red;">Pointers in expressions</mark>

**int value = 999;**

**pnumber = \&value;**

**\*pnumber += 25;** → The value of "value" variable is incremented by 25.

#### <mark style="color:purple;">Example:</mark>

```c
long num1 = 0l;
long num2 = 0l;
long *pnum = NULL;

pnum = &num1;   // get address of num1
*pnum = 2l;     // set num1 to 2
pnum = &num2;   // get address of num2
++*pnum;        // encriment num2 indirectly

printf("num1 = %ld num2=%ld *pnum = %ld *pnum + num2 = %ld\n", num1, num2, *pnum, *pnum+num2);
```

## <mark style="color:red;">**P**</mark><mark style="color:red;">ointers and const</mark>

We can use the const keyword when declaring a pointer to indicate that the value pointed to must not be changed.

```c
long value = 999l; 
const long *pvalue = &value;
```

The compiler will check for any statements that attempt to modify the value pointed to by pvalue and flag such statements as an error.

## <mark style="color:red;">Pointers to constants</mark>

We can still modify value, we have only applied const to the pointer not the variable itself.

```c
value=7777l;
```

#### <mark style="color:purple;">The value pointed to has changed but we didn't use the pointer to make the change. the pointer itself is not constant so we can still change what it points to:</mark>

```c
long number = 888l; 
pvalue = &number;
```

#### <mark style="color:purple;">We can create a constant pointer that points to a value that is also constant:</mark>

```c
 int item = 25;
 const int *const pitem = &item;
```

Now even though item variable is not defined as constant but the \*const pointer makes it constant and we cant change the pointer or variable values.

## <mark style="color:red;">Void pointers</mark>

The type name void means absence of any type a pointer of type void can contain the address of data item of any type.&#x20;

Void is often used as a parameter type or return value type with functions that deal with data in a type-independent way.

Any kind of pointer can be passed around as a value of type void the void pointer can be doesn't know what type of object it is pointing to so it cant be de-referenced directly the void pointer must first be explicitly cast to another pointer type variable of type void.

When we want to access the integer value at the address stored in the void pointer, we must first cast the pointer to type int.

#### <mark style="color:purple;">Example:</mark>

```c
int i = 10;
float f = 3.34;
char ch = 'k';

void *vptr;

vptr = &i;
printf("value of i=%d\n", *(int *) vptr);

vptr = &f;
printf("value of f=%f\n", *(float *) vptr);

vptr = &ch;
printf("value of ch=%c\n", *(char *) vptr);
```

## <mark style="color:red;">Pointers and arrays</mark>

An array is a collection of objects of the same type that you can refer to using a single name we can use a pointer to hold the address of different variables at different times(must be same type) arrays and pointers seem quit different but they are very closely related and can sometimes be used interchangeably.

The main reason for using pointers to arrays are ones of notional convenience and of program efficiency, pointers to arrays generally result in code that uses less memory and executes faster.

In pointers to arrays we point to each value of the array specifically and not the whole array at once.

<pre class="language-c"><code class="lang-c"> int values[100];
 int *pvalues;
 pvalues = values;  // point to the first element of the values array
<strong> // or
</strong> pvalue  = &#x26;values[0]
 pvalue  = &#x26;values[1] → second element
 pvalue  = &#x26;values[1] → third element
</code></pre>

#### <mark style="color:purple;">Example:</mark>

```c
int values[100]={[1]=324};
int *pointer;

pointer = &values[1];

printf("value 1 : %d\n", *pointer);
 
```

If "ar" is an array the two expressions ar\[i] and \*(ar+i) are equivalent in meaning, both work if ar is the name of an array, both work if ar is a pointer variable using an expression such as ar++ only works if ar is a pointer variable.

## <mark style="color:red;">Pointer arithmetic</mark>

```c
int values[100];
int *pvalues;

pvalues = values; 
```

To reference values\[3] through the pvalues variable we can add 3 to pvalues and then apply the indirection operator.

```c
*(pvalues + 3)
```

```c
int values[100]={[3]=352};
int *pointer;

pointer = &values;

printf("value 3 : %d\n",*(pointer+3));
```

This expression can be used to access the value contained in values\[i].

#### <mark style="color:purple;">For example to set vales\[10] to 27:</mark>

<pre class="language-c"><code class="lang-c">values[10] = 27;
<strong>// or
</strong>*(pvalues + 10) = 27;
</code></pre>

```c
*(pointer+3) = 321;
printf("set value 3 to : %d\n", *(pointer+3)); 
```

The increment and decrement operators ++ and -- are particularly useful when dealing with pointers.

**++pvalues;** → sets pvalues pointing to the next integer in the values array (values\[1]).

**--pvalues;** → sets pvalues pointing to the previous integer in the values array assuming that pvalues was not pointing to the beginning of the values array.

#### <mark style="color:purple;">Example:</mark>

```c
// loop trough an array and get a sum using an array

#include <stdio.h>

int sum( int ar[], int size);

int main(){

    int array[] = {23,67,3,46,25,5};
    
    printf("%d\n", sum(array, 6));
    
    return 0;
}

int sum(int ar[], int size){

    int s = 0;
    
    for(int i = 0; i < size; i++)
        s += ar[i];
        
    int avg = s/size;
    
    return avg;
}
```

#### <mark style="color:purple;">Example:</mark>

```c
// loop trough an array and get a sum using a pointer (less code)

#include <stdio.h>

int sum( int *ar, int size);

int main(){

    int array[] = {23,67,3,46,25,5};
    
    printf("%d\n", sum(array, 6));
    
    return 0;
}

int sum(int *ar,  int size){

    int s = 0;
    
    for(int i = 0; i < size; i++)
        s += *(ar+i);
        
    int avg = s/size;
    
    return avg;
}
```

#### <mark style="color:purple;">Example assigning and resolving array using pointers:</mark>

```c
#include <stdio.h>

int main(){

    int size=0, counter=0;
    
    printf("\n Enter the size of the array: ");
    
    scanf("%d", &size);
    int array[size];
    int *ptr = array;

    while(counter < size ){
        printf("\n Enter element number %d : ", counter);
        scanf("%d",&(*(ptr+counter)));
        counter += 1;
    }

for (int j=0; j < size; j++){
    printf("\n Element number %d: %d\n", j, *(ptr+j));
}
    return 0;
}
```

## <mark style="color:red;">Pointers and strings</mark>

If text is an array of characters we can define a pointer to be used to point to elements in text.

```c
char text[] = "a string";
char *ptext=&text;

++ptext; → go to next character in the array
```

#### <mark style="color:purple;">Example:</mark>

```c
 void copyString(char *to, char *from){

    while(*from)  // NULL character is equal to value 0 (false) so it will jump out
        *to++ = *from++;
        *to = '\0';
}

int main(void){

    char string1[]= "a string to be copied.";
    char string2[50];

    copyString(string2, string1);
    printf("%s\n", string2);
} 
```

#### <mark style="color:purple;">Example:</mark>

```c
 // A program to take string length without strlen function
 
int stringLength(const char *string){

    const char *lastAddress = string;
    
    while (*lastAddress)
        ++lastAddress;
        return lastAddress - string;}

int main(){

    printf("%d\n", stringLength("This is a test"));
    printf("%d\n", stringLength("This is another test"));
    printf("%d\n", stringLength("And another one :)"));
}
```

## <mark style="color:red;">Pass by reference</mark>

#### <mark style="color:purple;">There are few ways to pass data to a function:</mark>

1. Pass by value.
2. Pass by reference.

#### <mark style="color:purple;">Example pass by value:</mark>

```c
void swap(int x, int y){

    int temp;
    
    temp = x;  // save the value of x
    x = y;     // put y into x
    y = temp;  // put temp into y
    
    return 0;
}
```

#### <mark style="color:purple;">Using pointers to pass data:</mark>

```c
 void swap(int *x, int *y){
 
    int temp;
    
    temp = *x;  // save the value at address x
    *x = *y;     // put y into x
    *y = temp;  // put temp into y
    
    return 0;
}
```

## <mark style="color:red;">**P**</mark><mark style="color:red;">assing data using copies of pointers</mark>

We can pass a pointer as an argument to a function and you can also have a function return a pointer as its result pass by reference copies the address of an argument into the formal parameter, the address is used to access the actual argument used in the call.

Means the changes made to the parameter affect the passed argument.

To pass a value by reference arguments pointers are passed to the functions just like an kind of value.&#x20;

We need to declare the function parameters as pointer types.

Changes inside the function are reflected outside the function as well.&#x20;

Unlike call by value where the changes do not reflect outside the function.

#### <mark style="color:purple;">Example:</mark>

```c
void copyString(char *to, char *from){

    while(*from)  // NULL character is equal to value 0 (false) so it will jump out
        *to++ = *from++;
        *to = '\0';
}

int main(void){

    char string1[]= " a string to be copied.";
    char string2[50];

    copyString(string2,string1);
    printf("%s\n",string2);
}
```

## <mark style="color:red;">Returning a pointer from a function</mark>

```c
 int * function(){
         ... 
 }
```

***

## <mark style="color:red;">Double pointers (pointer to a pointer)</mark>

As we are talked about de-referencing, there can be a reference for every variable and data type, including a pointer which is a reference to another variable itself.&#x20;

A pointer which points to another pointer is called a double pointer and is works just like a normal pointer but it contains the address of a pointer instead of a variable.

<figure><img src="../../.gitbook/assets/image (42).png" alt="" width="563"><figcaption></figcaption></figure>

```c
 int **pointer;
```

<figure><img src="../../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (56) (1).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (76).png" alt="" width="563"><figcaption></figcaption></figure>

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>

int main(){

    int num = 123;
    int *singlep = NULL;
    int **doublep = NULL;

    singlep = &num;
    doublep = &singlep;

    printf("num address: %p\n", &num);
    printf("single address: %p\n", &singlep);
    printf("double address: %p\n", &doublep);

    printf("single value: %p\n", singlep);
    printf("double value: %p\n", doublep);

    printf("double reference: %p\n", *doublep);
    printf("single reference: %d\n", *singlep);
    printf("double reference from single: %d\n", **doublep);
}
```

## <mark style="color:red;">Use cases</mark>

The biggest reason to use double pointers is when we need to change the value of the pointer passed to a function as the function argument.&#x20;

Simulate pass by reference.

If you pass a single pointer in as argument you will be modifying local copies of the pointer not the original pointer in the calling scope.&#x20;

With a pointer to a pointer you modify the original pointer.

We use a double pointer as an argument to a function when we want to preserve the memory-allocation or assignment even outside of the function.

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>
#include <malloc.h>

void foo(int *ptr){

    int a = 5;
    ptr = &a;
}

int main() {

int *ptr = NULL;

ptr = (int *)malloc(sizeof(int));

*ptr = 10;

foo(ptr);

printf("%d\n",*ptr);

    return 0;
}
```

In the example above the final printed value will still be 10 because the pointer that we used in the function parameter is locally assigned and like a regular variable this pointer has a local value.

When we define a pointer with the same name inside the main function we are giving it a new value so even after calling the foo function that value wont change so the output will be the value defined inside the main function.

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>
#include <malloc.h>

void foo(int **ptr){

    int a = 5;
    *ptr = &a;
}

int main() {

int *ptr = NULL;

ptr = (int *)malloc(sizeof(int));

*ptr = 10;

foo(&ptr);

printf("%d\n",*ptr);

    return 0;
}
```

{% hint style="info" %}
This prints out 5.
{% endhint %}

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>
#include <string.h>
#include <malloc.h>

void foo(char **ptr){

    *ptr = malloc(255);
    strcpy(*ptr,"hello world");
}

int main() {

char *ptr = NULL;

foo(&ptr);

printf("%s\n",ptr);

free(ptr);

    return 0;
}
```
