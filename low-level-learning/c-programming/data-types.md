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

# Data Types

## <mark style="color:red;">Integer</mark>

**int** → 4.\
**float** → 12.585, -.0001 ( stores less values than double).\
**double** → same as float but stores twice the precision.

{% hint style="info" %}
**To explicitly express a float constant, append either an f or F to the end of the number e.g: 12.54f**
{% endhint %}

## <mark style="color:red;">Boolean</mark>

Boolean data type for storing 0 or 1 values.

Used for on/off, yes/no, true/false situation (binary choices).

{% hint style="info" %}
Include **\<stdbool.h>** to use bool instead of \_Bool.
{% endhint %}

<pre class="language-c"><code class="lang-c">#include &#x3C;stdbool.h>

bool var = true;
   
// Or we can do:

    _Bool y = 1; // True
    _Bool n = 0; // False


<strong>// Or use preprocessors (have to include stdbool header):
</strong>
#define true 1
#define false 0

#define yes true
#define no false
</code></pre>

## <mark style="color:red;">size\_t</mark>

**size\_t** → used to represent the size of objects in bytes and is therefore used as the return type by the sizeof operator.&#x20;

Size\_t is a type guaranteed to hold any array index only for non-negative values.

```c
size_t s1 = strlen(str1);
printf("%zu", s1);
```

## <mark style="color:red;">wchar\_t</mark>

**wchar\_t** → Wide char is similar to char data type, except that wide char take up twice the space and can take on much larger values as a result.

Char can take 256 values which corresponds to entries in the ASCII table.

On the other hand, wide char can take on 65536 values which corresponds to UNICODE values which is a recent international standard which allows for the encoding of characters for virtually all languages and commonly used symbols.

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    wchar_t *s;

    s = (wchar_t *)malloc(sizeof(wchar_t) * 2);
    s[0] = 42;   // ascii code for *
    s[1] = 115;   // ascii code for 's'
    
    printf("%ls\n", s);
    free(s);
    
    return (0);
}
```

## <mark style="color:red;">Enum Type</mark>

They are arithmetic types and they are used to define variables that can only assign certain discrete integer values throughout the program.

```c
enum var { value1, value2, value3 };
```

The compiler actually treats enumeration identifiers as integer constants. first name in list is 0

```c
enum month thisMonth;
thisMount = February;
```

The value 1 is assigned to thisMonth and not the name February because it is the second identifier listed inside the enumeration list.

If you want to have a specific integer value associated with an enumeration identifier the integer can be signed to the identifier when the data type is defined.

```c
enum direction { up, down, left=10, right};
                 1    2     10        11
```

#### An enumerated data type direction is defined with the values up, down, left, right up = 0 because it appears first in the list down = 1 cause its next left = 10 cause we said so right = 11 cause it appears immediately after left in the list.

## <mark style="color:red;">char</mark>

Represents a single character such as the letter 'a'. always uses single quotes.

**char var;** → a single character

```c
var='t';
```

char grade = 65; → is valid for ASCII code but not recommended.

## <mark style="color:red;">Escape Characters</mark>

```c
 \a → alert
 \b  → backspace
 \f → form feed
 \n → new line
 \r → carriage  return
 \t → horizental tab
 \v → vertical tab
 \\ → backslash \
 \' → single quote
 \" → double quote
 \? → question mark ?
 \0oo → octal value. o represents an octal digit
 \xhh → hexadecimal value. h represents a hexadecimal digit
```

## <mark style="color:red;">Data Type Conversion Functions</mark>

{% code overflow="wrap" %}
```c
 double a = atof(s) →  converts the string into a floating-point number, returning the result
 
 int a = atoi(s) → converts the string into an int, returning the result
 
 int a = atol(s) → converts the string into a long int, returns result
 
 int a = atoll (s) → converts string into long long int, returns result 
```
{% endcode %}

## <mark style="color:red;">Printing Format Strings</mark>

<figure><img src="../../.gitbook/assets/image (7).png" alt="" width="536"><figcaption></figcaption></figure>

## <mark style="color:red;">Complex Numbers</mark>

A complex number is a number of the form A + Bi is is the square root of minus one (imaginary) a and b are real numbers.

C11 doesn't support complex numbers so it implements the macro **STDC\_NO\_COMPLEX** in the header section.

#### <mark style="color:purple;">To test whether your compiler supports complex or not:</mark>

```c
#ifdef __STDC_NO_COMPLEX__
printf("complex arithmetic is not supported");
#else
printf("complex is supported");
#endif
```

#### <mark style="color:purple;">Declaring complex numbers:</mark>

```c
 double _Complex ;
 float_Complex;
 long double_Complex;
```

#### <mark style="color:purple;">If we include the complex.h header we can do it this way:</mark>

```c
 double complex z1;
```

## <mark style="color:red;">Complex Functions</mark>

**creal()** → returns the real part of a value of type double complex that is passed as the argument.

**cimage()** → returns the imaginary part.

```c
 double real_part = creal(z1);
 double imag_part = cimage(z1);
```

we append an f to these functions names when we are working with float complex values (crealf() and cimagef()) and a lowercase l when using with long complex ( creall() and cmagl())c

**conj()** → returns the complex conjugate of its double complex.

**conjf() and conjl()** → return the complex conjugate for the other two types long and float.

## <mark style="color:red;">Imaginary Numbers</mark>

#### <mark style="color:purple;">We can use \_imaginary keyword to define variables that store purely imaginary numbers.</mark>

```c
  double _Imaginary ix = 2.4*l;
  float_Imaginary;
```

#### <mark style="color:purple;">if we include the complex.h header we can do it this way:</mark>

```
 double imaginary ix = 2.4*l
```

## <mark style="color:red;">typedef</mark>

A keyword that allows us to create our own name for an existing data type.&#x20;

Defines the name counter to be equal to the C data type int.&#x20;

#### <mark style="color:purple;">Now variables can be declared of type counter:</mark>

```c
 typedef int counter;
```

## <mark style="color:red;">Operators</mark>

<pre class="language-c"><code class="lang-c">+ → add  
- → subtract
* → multiply
/ → divide
% → remainder of division
++ → increment by one
-- → decrement by one
= → simple assign operator
+= → add and assign
-= → subtract and assign
*= → multiply and assign
/= → devide and assign
<strong>%= → modulus and assign
</strong>&#x3C;&#x3C; → left shift operator, i.e: 1 &#x3C;&#x3C; 0 == 2 to the power of 0 == 1 == binary 0001
>> → right shift operator
>>= → right shift and assign
&#x3C;&#x3C;= → left shift and assign
&#x26;= → bitwise and assign
^= → bitwise exclusive or and assign
|= → bitwise inclusive or and assign
</code></pre>

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>

int main(){
    unsigned int a = 60; // 0011 1100
    unsigned int b = 13; // 0000 1101
    int result = 0;
    int c = a & b; // 0000 1100
    printf("result : %d", c);
    return 0;
}
```

## <mark style="color:red;">Difference between x++ and ++x</mark>

**++x** happens prior to assignment (per-increment)

**x++** happens after assignment (post-increment).&#x20;

**x++** executes the statement and then increments the value.&#x20;

**++x** increments the value and then executes the statement.

## <mark style="color:red;">logical Operators</mark>

**&&** → AND operator, if both are non-zero the condition is true (A && B) is false.

**||** → OR operator, if any or both are non-zero the condition is true (A || B) is true.

**!** → NOT operator, reverse the logical statement !(A && B) is true.

```c
 (A || B) ? printf("true") : printf("false");
 (A && B) ? printf("true") : printf("false");
```
