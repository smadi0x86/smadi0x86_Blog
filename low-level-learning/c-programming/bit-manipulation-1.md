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

# Bit Manipulation

Bit manipulation is the act of algorithmically manipulating bits or other pieces of data shorter than a word.

#### <mark style="color:purple;">Tasks that require bit manipulation:</mark>

* Low-level device control.
* Error detection.
* Correction algorithms.
* Data compression.
* Encryption.
* Algorithms optimization.

## <mark style="color:red;">Binary numbers system</mark>

#### <mark style="color:purple;">Each position value in a binary number are the powers of two:</mark>

```c
 128      64      32        16          8           4         2       1
 2^7     2^6     2^5        2^4        2^3         2^2        2^1    2^0
```

<figure><img src="../../.gitbook/assets/image (35).png" alt="" width="478"><figcaption></figcaption></figure>

{% hint style="info" %}
1 byte = 8 bits. &#x20;

0 = off, 1 = on.
{% endhint %}

The rightmost bit of a byte is known as the least significant or low-order bit, the leftmost bit is known as the most significant or high-order bit.

Changing a bit to 1 is called setting a bit changing a bit to 0 is called resetting a bit.

## <mark style="color:red;">Bits for basic C data types</mark>

```c
 BIT            _Bool           1            0 to 1
 Byte           char            8           -128 to 127 → ACSII table
 word           short int       16          -32768 to 32767
 long           long int        32          -2147483648 to 2147483647
```

## <mark style="color:red;">Negative numbers (signed)</mark>

#### <mark style="color:purple;">The representation of negative number:</mark>

```c
 computer represents such numbers using a "twos complement" notation:
 the leftmost bit represents the sign bit
 
 if its 1 number is negative
 if its 0 number is positive
```

A way to convert negative numbers from decimal to binary is to first add 1 to the value express the absolute value of the result in binary complement all the bits change all the 1s to 0s and 0s to 1s.

#### <mark style="color:purple;">Example:</mark>

```
To convert -5 to binary 1 is added which gives -4.
4 expressed in binary is 00000100.
Complementing the bits produces 11111011.
```

#### <mark style="color:purple;">Example:</mark>

```c
 // convert decimal to binary
 
#include <stdio.h>
#include <math.h>

long long converter(int n){
long long bin = 0;

int remainder, i=1;

while(n != 0){
    remainder = n % 2;
    n = n / 2;
    bin += remainder * i;
    i *= 10;}
    
    return bin;
}

int main(){
    int n = 0;
    long long result = 0;
    
    printf("Enter a decimal number: ");
    scanf("%d",&n);
    
    result = converter(n);
    
    printf("%d in decimal = %ld to binary",n,result);
    return 0;
}
```

#### <mark style="color:purple;">Example:</mark>

```c
// converting binary to decimal
 
#include <math.h>
#include <stdio.h>

long long converter(long long n){

    int dec = 0, i = 0 , remainder = 0;
    while (n != 0){
        remainder = n % 10;
        n = n / 10;
        dec += remainder*pow(2,i);
        ++i;}
        
    return dec;
}
int main() {
    long long n = 0;
    int result = 0;
    
    printf("Enter a binary number: ");
    scanf("%lld", &n);
    
    result = converter(n);
    
    printf("%lld in binary = %d in decimal", n, result);
    
    return 0;
}
```

#### <mark style="color:purple;">If you see the error about pow() in IDE You need to link with the math library:</mark>

```bash
gcc -o sphere sphere.c -lm
```

{% hint style="danger" %}
Error: ld returned 1 exit status is from the linker ld (part of gcc that combines the object files) because it is unable to find where the function pow is defined.
{% endhint %}

Including math.h brings in the declaration of the various functions and not their definition.&#x20;

You need to link your program with this library so that the calls to functions like pow() are resolved.
