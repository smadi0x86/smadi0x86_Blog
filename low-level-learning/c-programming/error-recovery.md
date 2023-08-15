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

# Files

## <mark style="color:red;">**File Structure in C**</mark>

* Files are continuous sequences of bytes ending with an EOF (End Of File) indicator.
* Current position denotes where read/write operations occur.
* Files can be in text or binary format.

## <mark style="color:red;">**File Handling Functions**</mark>

#### <mark style="color:purple;">**Opening a File**</mark><mark style="color:purple;">:</mark>

```c
FILE *fopen(const char* restrict name, const char *restrict mode);
```

#### <mark style="color:purple;">**Renaming a File**</mark><mark style="color:purple;">:</mark>

```c
int rename(const char *oldname, const char *newname);
```

#### <mark style="color:purple;">**Closing a File**</mark><mark style="color:purple;">:</mark>

```c
int fclose(FILE *filePtr);
```

#### <mark style="color:purple;">**Deleting a File**</mark><mark style="color:purple;">:</mark>

```c
int remove(const char* filename);
```

#### <mark style="color:purple;">**Reading Characters from a File**</mark><mark style="color:purple;">:</mark>

```c
int fgetc(FILE *filePtr);
int getc(FILE *filePtr);
```

#### <mark style="color:purple;">**Reading Strings from a File**</mark><mark style="color:purple;">:</mark>

```c
char *fgets(char *str, int nchars, FILE *stream);
```

#### <mark style="color:purple;">**Reading Formatted Input**</mark><mark style="color:purple;">:</mark>

```c
int fscanf(FILE *stream, const char *format, ...);
```

#### <mark style="color:purple;">**Writing Characters to a File**</mark><mark style="color:purple;">:</mark>

```c
int fputc(int c, FILE *filePtr);
int putc(int c, FILE *filePtr);
```

#### <mark style="color:purple;">**Writing Strings to a File**</mark><mark style="color:purple;">:</mark>

```c
int fputs(const char *str, FILE *stream);
```

#### <mark style="color:purple;">**Writing Formatted Output**</mark><mark style="color:purple;">:</mark>

```c
int fprintf(FILE *stream, const char *format, ...);
```

#### <mark style="color:purple;">**Miscellaneous Functions**</mark><mark style="color:purple;">:</mark>

* Checking for end of file: `int feof(FILE *filePtr);`
* Flushing data to file: `int fflush(FILE *filePtr);`
* Get current file position: `int fgetpos(FILE *stream, fpos_t *pos);`
* Set file position: `int fseek(FILE *stream, long int offset, int whence);`
* Get current position in file: `long ftell(FILE *stream);`

#### <mark style="color:purple;">**Example of Repositioning File Stream**</mark><mark style="color:purple;">:</mark>&#x20;

Using `fseek()`, you can move the file pointer to a desired location.

#### <mark style="color:purple;">**Writing/Reading Integers**</mark><mark style="color:purple;">:</mark>

&#x20;Use `putw()` to write and `getw()` to read.

#### <mark style="color:purple;">**Flushing a File**</mark><mark style="color:purple;">:</mark>&#x20;

`fflush()` flushes the output buffer of a file stream.
