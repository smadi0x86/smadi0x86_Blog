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

# Strings Manipulation

## <mark style="color:red;">String representation</mark>

#### <mark style="color:purple;">char word\[7] = {"hellow"};</mark> → always use double quotes and char type.

or

#### <mark style="color:purple;">char word\[7] = "hellow";</mark> → can remove brackets.

or

#### <mark style="color:purple;">char word\[] = "hellow";</mark> → this one is better.

## <mark style="color:red;">Reversing a string</mark>

```c
#include <stdio.h>
#include <string.h>

void reverse(char*, int, int);

int main()
{
    char a[100];
    
    gets(a);
    reverse(a, 0, strlen(a)-1);
    
    printf("%s\n", a);
    
    return 0;
}

void reverse(char *x, int begin, int end)
{
    char c;
    if (begin >= end)
        return;
        
    c = *(x+begin);
    *(x+begin) = *(x+end);
    *(x+end)   = c;
    
    reverse(x, ++begin, --end);
}
```

## <mark style="color:red;">Analyzing strings</mark>

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>
#include <ctype.h>

int main(){

    char a[100] = "hello world";

    for (int i = 0; a[i] != '\0'; i++ ){
        if(islower(a[i]))
            printf("lower: %c\n",a[i]);
    }


    return 0;
}
```

## <mark style="color:red;">Finding mixed character strings (without string.h)</mark>

```c
#include <stdio.h>
#include <stdbool.h>

int main(){

    char str[] = "Hello World";
    int i;
    char  found_lower = false, found_upper = false;

    for (int i = 0; str[i] != '\0'; i++) {
        found_lower = found_lower || (str[i] >= 'a' && str[i] <= 'z');
        found_upper = found_upper || (str[i] >= 'A' && str[i] <= 'Z');

        if (found_lower && found_upper) break;
    }

    printf("%d",found_lower && found_upper);

    return 0;
}
```

## <mark style="color:red;">Summary</mark>

```c
strlen(a)                → get the length of a

strncat(a,b,sizeOfA)     → append b to a 

strchr(a,'a')            → search for first occurence of 'a' in  string a

strrchr(a,'a')           → search for last occurence of 'a' in string a

strstr(a,b)              → search for string b in a

strncmp(a,b,sizeOfA)     → compare strings a and b

strncpy(a,b,sizeOfA)     → copy b to a (overwrite a)

memcmp(a,b,FirstNBytesToCompare) → compare first n bytes

memchr(a,'a',sizeOfA)  → search array a for character 'a'

strtok(a,t)            → tokenize string a by string t
```
