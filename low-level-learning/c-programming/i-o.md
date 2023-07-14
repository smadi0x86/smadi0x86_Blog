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

# I/O

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

## <mark style="color:red;">Char functions (input)</mark>

**int getc(FILE \*stream);** → read a single character from a file.

#### <mark style="color:purple;">Example read from a file:</mark>

```c
#include <stdio.h>

int main(){
    char ch = '\0';
    FILE *fp;
    
    if (fp = fopen("test","rw")){
        ch = getc(fp);
        while (ch != EOF)
        {
            printf("%c",ch);
            ch = getc(fp);
        }
        
        fclose(fp);
    }
    return 0;
}
```

#### <mark style="color:purple;">Example read from stdin ( i.e: keyboard):</mark>

```c
int main(){
        char ch = '\0';
        
        ch = getc(stdin);
        
        printf(">>> %c\n", ch);
        
        return 0;
}
```

#### <mark style="color:purple;">We can do it this way too:</mark>

```c
int main(){
    int ch = 0;
    
    while ((ch = getchar()) != EOF)
    
    printf("%c\n", ch);
    
    return 0;
}
```

#### <mark style="color:purple;">We can do it with catching spaces:</mark>

```c
int main(){
    int ch = 0;
    
    while (isspace(ch = (char)getchar()));
    
    printf("%c\n", ch);
    
    return 0;
}
```

**int ungetc(int char, FILE \*stream);** → pushes the character char (an unsigned char) into the specified stream so that this is available for the next read operation.

```c
#include <stdio.h>

int main () {
    FILE *fp;
    int c;
    char buffer [256];

    fp = fopen("test", "r");
    
    if( fp == NULL ) {
        perror("Error in opening file");
        return(-1);
    }
    
    while(!feof(fp)) {7h3w4lk3r/The-Hive
        c = getc (fp);
        /* replace ! with + */
        if( c == '!' ) {
            ungetc ('+', fp);
        } else {
            ungetc(c, fp);
        }
        fgets(buffer, 255, fp);
        fputs(buffer, stdout);
    }
    return(0);
}
```

## <mark style="color:red;">Char functions (output)</mark>

**int putc(int char, FILE \*fp)** → write a single character to a file or stdout.

```c
putc('\n', stdout);
```

#### <mark style="color:purple;">Takes 2 args:</mark>

1. Character
2. File Pointer

#### <mark style="color:purple;">Example redirecting the input to a file:</mark>

<pre class="language-c"><code class="lang-c">int main(){
int ch = 0;
while ((ch = getchar()) != EOF)
    putchar(ch);
    ungetc(ch, stdin); // returns EOF previously read back to stdin
    
<strong>    printf("EOF signal detected!\n");
</strong>    return 0;
}
</code></pre>

#### <mark style="color:purple;">Use the above program like this:c</mark>

```c
./main < infile
```

**int fputc(int character, FILE \*stream);** → writes a character (an unsigned char) to the specified stream and advances the position indicator for the stream.

#### <mark style="color:purple;">Example write characters a-z in a file:</mark>

```c
int main(){
FILE *pfile = NULL;
char c = '\0';

pfile = fopen("test","w");

if (pfile != NULL){
    for (c = 'A'; c <= 'Z'; c++)
    
    fputc(c,pfile);
    fclose(pfile);
}
    return 0;
}
```

#### <mark style="color:purple;">Example count the number of characters and words in a file or from stdin:</mark>

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]){
    FILE * fp = NULL;
    char ch = '\0';
    int wrd = 1, character = 1;

    if (argc==1)
        fp = stdin;
    else if (argc == 2){
    
        if ((fp = fopen(argv[1], "r")) == NULL){
            fprintf(stderr, "cant open the file %s\n", argv[1]);
            exit(EXIT_FAILURE);
        }
    } else {
        fprintf(stderr, "usage: %s [filenme]\n", argv[0]);
        exit(EXIT_FAILURE);
    }

    ch = getc(fp);
    
    while (ch != EOF){
        if (ch ==' ' || ch=='\n'){
            wrd++;
        } else {
            character++;
        }

        ch = getc(fp);
    }
    
printf("\n the number of words : %d\n", wrd-1);
printf("the number of characters: %d\n", character-1);

return 0;
}
```

{% hint style="info" %}
#### To enter EOF press Ctrl+D keys
{% endhint %}

#### <mark style="color:purple;">Example convert uppercase to lowercase and vice versa in a file and stdin:</mark>

```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

void convertCase(FILE * fptr, const char *path);

int main(){
    FILE * fptr = NULL;
    char path[100];
    
    printf("enter the full path to the file: ");
    scanf("%s", path);
    
    fptr = fopen(path, "r");
    
    if (fptr == NULL){
        fprintf(stderr, "unable to open file. \n");
        exit(EXIT_FAILURE);
    }
    
    convertCase(fptr,path);
    return 0;
}

void convertCase(FILE * fptr, const char *path){

    FILE *dest = NULL;
    char ch = '\0';
    
    dest = fopen("output","w");
    
        if (dest == NULL){
        fprintf(stderr, "unable to open file. \n");
        exit(EXIT_FAILURE);
        }
        
        while ((ch = fgetc(fptr)) != EOF){
            if (isupper(ch))
                ch = tolower(ch);
            else if (islower(ch))
                ch = toupper(ch);
            fputc(ch,dest);
        }      
fclose(fptr);
fclose(dest);
remove(path);
rename("output",path);
}
```

## <mark style="color:red;">String functions (input)</mark>

#### ssize\_t getline(char _\*buffer,size\_t_ size, FILE \*stream);

**buffer →** a pointer to a block allocated with malloc or calloc (type char \*\*).

There is never a shortage of space cause it automatically enlarge the block of memory using realloc as needed (getline is safe) returns the line read by getline.

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(){
    char *buffer = NULL;
    size_t bufsize = 32;
    size_t characters;
    
    buffer = (char *)malloc(bufsize * sizeof(char));
    
    if (buffer == NULL){
        exit(1);
    }
    
    printf("type something: ");
    
    characters = getline(&buffer, &bufsize, stdin);
    
    printf("%zu characters were read\n", characters);
    printf(" you typed: %s\n", buffer);
    
    return 0;
}
```

**int fscanf(FILE \_fp, const char**\_\*\* format \[,argument,...]);\*\* → same as printf but on a file.&#x20;

Returns the number of arguments that are successfully read and assigned (on success), returns EOF if the end of the file is reached before any of the conversion specifications have been processed.

**fscanf(myFile, "%i", \&i);** → reads the next integer value from the file "myFile" and stores it in the variable i.

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>

int main() {
    FILE *fp;
    char buff[255];
    
    fp = fopen("test","r");
    
    while (fscanf(fp,"%s",buff)!=EOF){
        printf("%s ", buff);
    }
    
    fclose(fp);
    return 0;
}
```

## <mark style="color:red;">Formatting functions</mark>

**int sprintf(char \_string, const char**\_\*\* format,...)\*\* → used to write formatted output to a string. we can combine several data variables into a character array, instead of printing on the console we store the output to a char buffer

**sprintf(string, "%d %c %f", value, c, fit);**

First parameter is a char pointer for output(buffer) the function returns the number of characters stored in the string.

{% hint style="info" %}
This function is unsafe and vulnerable to buffer overflow.
{% endhint %}

```c
#include <stdio.h>

int main() {
    char string[100];
    int a = 10, b=20, c= 0;
    c = a + b;
    
    sprintf(string, "sum of %d and %d is %d", a, b, c);
    puts(string);
    
    return 0;
}
```

**sscanf(const char \_str, const char**\_\*\* control\_string \[arg1,arg2,...]); → allows to read formatted data from a string rather than stdin or keyboard.

**sscanf(buffer,"%s %d",  name, \&age);**

First arg is a pointer to string from where to read the data returns the number of items read from the string and -1 if an error is encountered.

#### <mark style="color:purple;">Example:</mark>

```c
#include <stdio.h>

int main() {
    char *str = "jason fedin 43";
    char name[10], title[10];
    int age, ret;

    ret = sscanf(str, "%s %s %d", name , title, &age);

    printf("name: %s\n",name);
    printf("title: %s\n",title);
    printf("age: %d\n",age);

  return ret;
}
```

<figure><img src="../../.gitbook/assets/image (41).png" alt="" width="563"><figcaption></figcaption></figure>

**fflush()** → used to flush/clean a file or buffer.

```c
int fflush(FILE *fp);

fflush(buffer);
```

#### <mark style="color:purple;">For float and double values we can print a specific width of the value like this:</mark>

```c
float floatValue = 432.476868734;

printf("float with width of 3: %.3f", floatValue);

// output: 432.476
```

{% hint style="info" %}
```
%.3f only print the first 3 sections.
```
{% endhint %}

#### <mark style="color:purple;">For integer input always use '&' before variable name:</mark>

```c
#include <stdio.h>
int main() {
    char str[100];
    int i;
    
    printf("Enter a value:");
    scanf("%d", &i);
    
    printf("\nYou entered: %d\n", i);   
    
    return 0;
    }
```

## <mark style="color:red;">Printing system error messages</mark>

**perror("error text");** → used for printing error messages followed by system stderr.

Returns the number of items that it successfully reads.&#x20;

While using scanf() to read a value for one of the basic variable types, prepend the variable name with an "&" sign .&#x20;

While using scanf() to read a string into a character array, don't use an "&" sign.
