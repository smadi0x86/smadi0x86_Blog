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

This guide discusses the C functions used for reading and writing characters and strings, with examples of their usage.

## <mark style="color:red;">**Character Functions for Input**</mark>

#### <mark style="color:purple;">**`getc`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**Function:**</mark>

* **Purpose**: Reads a single character from a file.
* **Syntax**: `int getc(FILE *stream);`

### <mark style="color:yellow;">**Examples**</mark>

#### <mark style="color:purple;">Reading from a file:</mark>

```c
#include <stdio.h>
int main() {
    char ch = '\0';
    FILE *fp;
    if ((fp = fopen("test","r")) != NULL) {
        while ((ch = getc(fp)) != EOF) {
            printf("%c",ch);
        }
        fclose(fp);
    }
    return 0;
}
```

#### <mark style="color:purple;">Reading from</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`stdin`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">(i.e., keyboard):</mark>

```c
int main() {
    char ch = getc(stdin);
    printf(">>> %c\n", ch);
    return 0;
}
```

#### <mark style="color:purple;">Alternative methods:</mark>

```c
int main() {
    int ch;
    while ((ch = getchar()) != EOF) printf("%c\n", ch);
    return 0;
}
```

#### <mark style="color:purple;">With spaces captured:</mark>

```c
int main() {
    int ch;
    while (isspace(ch = (char)getchar()));
    printf("%c\n", ch);
    return 0;
}
```

#### <mark style="color:purple;">**`ungetc`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**Function**</mark><mark style="color:purple;">:</mark>

* **Purpose**: Pushes a character back into the stream.
* **Syntax**: `int ungetc(int char, FILE *stream);`

#### <mark style="color:purple;">**Example**</mark><mark style="color:purple;">:</mark>

```c
#include <stdio.h>
int main() {
    FILE *fp;
    int c;
    char buffer[256];
    fp = fopen("test", "r");
    while (!feof(fp)) {
        c = getc(fp);
        if (c == '!') ungetc('+', fp);
        else ungetc(c, fp);
        fgets(buffer, 255, fp);
        fputs(buffer, stdout);
    }
    return 0;
}
```

## <mark style="color:red;">**Character Functions for Output**</mark>

#### <mark style="color:purple;">**`putc`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**Function**</mark><mark style="color:purple;">:</mark>

* **Purpose**: Writes a single character to a file or stdout.
* **Syntax**: `int putc(int char, FILE *fp);`
* **Usage**: `putc('\n', stdout);`

### <mark style="color:yellow;">**Example**</mark>

#### <mark style="color:purple;">Redirecting input to a file:</mark>

```c
int main() {
    int ch;
    while ((ch = getchar()) != EOF) putchar(ch);
    ungetc(ch, stdin);
    printf("EOF signal detected!\n");
    return 0;
}
```

#### <mark style="color:purple;">Use the program like:</mark>&#x20;

`./main < infile`

#### <mark style="color:purple;">**`fputc`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**Function**</mark><mark style="color:purple;">:</mark>

* **Purpose**: Writes a character to the specified stream.
* **Syntax**: `int fputc(int character, FILE *stream);`

### <mark style="color:yellow;">**Example**</mark>&#x20;

#### <mark style="color:purple;">Writing characters a-z to a file:</mark>

```c
int main() {
    FILE *pfile = NULL;
    char c;
    pfile = fopen("test", "w");
    for (c = 'A'; c <= 'Z'; c++) fputc(c, pfile);
    fclose(pfile);
    return 0;
}
```

## <mark style="color:red;">**String Functions for Input**</mark>

#### <mark style="color:purple;">**`getline`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**Function**</mark><mark style="color:purple;">:</mark>

* **Purpose**: Reads a line from the specified stream.
* **Syntax**: `ssize_t getline(char **buffer, size_t *size, FILE *stream);`

### <mark style="color:yellow;">**Example**</mark>

#### &#x20;<mark style="color:purple;">Reading a line with</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`getline`</mark><mark style="color:purple;">:</mark>

```c
#include <stdio.h>
#include <stdlib.h>
int main() {
    char *buffer = NULL;
    size_t bufsize = 32;
    size_t characters;
    buffer = malloc(bufsize * sizeof(char));
    if (buffer == NULL) exit(1);
    printf("Type something: ");
    characters = getline(&buffer, &bufsize, stdin);
    printf("%zu characters were read.\n", characters);
    printf("You typed: %s\n", buffer);
    return 0;
}
```

#### <mark style="color:purple;">**`fscanf`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**Function**</mark><mark style="color:purple;">:</mark>

* **Purpose**: Reads formatted input from a file.
* **Syntax**: `int fscanf(FILE *fp, const char *format, ...);`

#### <mark style="color:purple;">**Example**</mark><mark style="color:purple;">:</mark>

```c
#include <stdio.h>
int main() {
    FILE *fp;
    char buff[255];
    fp = fopen("test", "r");
    while (fscanf(fp, "%s", buff) != EOF) {
        printf("%s ", buff);
    }
    fclose(fp);
    return 0;
}
```

## <mark style="color:red;">**Formatting Functions**</mark>

#### <mark style="color:purple;">**`sprintf`**</mark><mark style="color:purple;">** **</mark><mark style="color:purple;">**Function**</mark><mark style="color:purple;">:</mark>

* **Purpose**: Writes formatted output to a string.
* **Syntax**: `int sprintf(char *str, const char *format, ...);`

#### <mark style="color:purple;">**Example**</mark><mark style="color:purple;">:</mark>

```c
#include <stdio.h>
int main() {
    char buffer[50];
    int a = 10, b = 20;
    sprintf(buffer, "%d plus %d is %d", a, b, a+b);
    printf("%s", buffer);
    return 0;
}
```
