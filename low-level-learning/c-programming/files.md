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

## <mark style="color:red;">File Structure in C</mark>

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

In C each file as a continuous sequence of bytes ending with an EOF (end of file ) indicator.

The current position is where any file action (read/write) will take place we can move the current position to any point in the file (even the end).

A text file is written as a sequence of characters organized as lines (each line ends with a newline).

Binary data is written as a series of bytes exactly as they appear in memory image data, music, video, not readable.

## <mark style="color:red;">Accessing Files</mark>

#### fopen() is defined in stdio.h header

```c
FILE *fopen(const char* restrict name, const char *restrict mode);
```

First arg is a pointer to a string that is the name of the external file.

The second arg is a character string that represents the file mode and specifies what you want to do with the file, a file mode specification is a character string between double quotes.

<figure><img src="../../.gitbook/assets/image (37).png" alt="" width="554"><figcaption></figcaption></figure>

{% hint style="info" %}
#### If the file doesn't exist FILE will return NULL;
{% endhint %}

## <mark style="color:red;">Write mode</mark>

#### <mark style="color:purple;">Example:</mark>

```c
   int main() {
    FILE *pfile = NULL;
    char *filename = "myfile.text";
    pfile = fopen(filename, "w"); // open myfile to write it
    if (pfile == NULL)
        printf("Failed to open file %s\n",filename);
}
```

## <mark style="color:red;">Append mode</mark>

```c
 pFile = fopen("myfile","a");
```

{% hint style="info" %}
#### Don't forget to check the value for NULL each time.
{% endhint %}

## <mark style="color:red;">Read mode</mark>

```c
pFile = fopen("myfile","r");
```

## <mark style="color:red;">Renaming a file</mark>

```c
int rename(const char *oldname, const char *newname)
```

The integer that is returned will be 0 if the name change was successful and nonzero otherwise.

{% hint style="info" %}
The file must not be open while renaming.
{% endhint %}

```c
if (rename("filename", "new_filename"))
    printf("failed to open file");
else
    printf("file renamed successfully"); 
```

## <mark style="color:red;">Closing a file</mark>

fclose() accepts a file pointer as an integer returns EOF (int) if an error occurs.

{% hint style="info" %}
If EOF was 0 the operation was successful.
{% endhint %}

```c
fclose(pfile); 
pfile = NULL;
```

## <mark style="color:red;">Deleting a file</mark>

remove() declared in \<stdio.h> header.

```c
 remove("myfile");
```

## <mark style="color:red;">Reading characters from a text file</mark>

fgetc() takes a file pointer as its only arg and returns the character read as type int.

#### int mchar = fgetc(pfile);

The mchar is type int because EOF will be returned if the end of the file has been reached.

Function getc() is available as well, it gets an argument of type FILE\* and returns the character read as type int virtually identical to fgetc() only difference between them is that getc() may be implemented as a macro whereas fgetc() is a function.

We can read the content of a file again when necessary the rewind() function positions to the file that is specified by the file pointer argument at the beginning.

#### rewind(pfile);

```c
// read the whole file
int main() {

FILE *fp;
int c;

fp = fopen("test","r");

if (fp == NULL) {
    perror("error in openning file");
    return(-1);
}

while((c = fgetc(fp)) != EOF)
        printf("%c", c);

fclose(fp);
fp = NULL;
return 0;
 }
```

## <mark style="color:red;">Reading a string from a text file</mark>

we can use the fgets() function to read from any file or stream.

#### char _fgets(char_ str, int nchars, FILE \*stream)

The function reads a string into the memory area pointed to by str, from the file specified by stream, characters are read until either a '\n' is read or nchars-1 characters have been read from the stream whichever occurs first.

If a newline character is read its retained in the string, a '\0' character will be appended to the end of the string.

If there is no error fgets() returns the pointer, str if there is an error, NULL is returned reading EOF causes NULL to be returned.

```c
// read 60 characters of a file
FILE *fp;
char str[60];
fp = fopen("test","r");

if(fp == NULL){
    perror("error opening file");
    return -1;
}

if (fgets(str, 60, fp)!= NULL){
    printf("%s", str);}

fclose(fp);
fp = NULL;
```

## <mark style="color:red;">Reading formatted input file</mark>

#### fscanf()

#### int fscanf(FILE _stream, const char_ format, ...)

#### <mark style="color:purple;">The first argument to this function is the format a C string that contains one or more of the following items:</mark>&#x20;

• White space character.&#x20;

• Non-white space character.&#x20;

• Format specifiers.

• Usage is similar to scanf but from a file.

#### Function returns the number of items successfully matched and assigned.

```c
FILE *fp;
char str1[10], str2[10], str3[10];
int year;

fp = fopen("test","w+");

if (fp != NULL)
    fputs("A B C ", fp);   // writing data to the file

rewind(fp);
fscanf(fp, "%s%s%s%d", str1, str2, str3, &year);  // reading parameters  from the file

printf("read string1: %s\n", str1);
printf("read string2 %s\n", str2);
printf("read string3 %s\n", str3);
printf("read integer %d\n", year);
fclose(fp);
```

## <mark style="color:red;">Writing characters to a text file</mark>

The simplest write operation is fputc() writes a single character to a text file.

First arg is the character to write, second one is the file pointer returns the character that was written if successful returns EOF if failure.

{% hint style="info" %}
#### putc() is the same as fputc() but its a macro.
{% endhint %}

```c
FILE *fp;
int ch;
fp = fopen("test","w+");

for (ch = 33; ch<=100; ch++){  // write some ascii values
    fputc(ch,fp);} 
    
fclose(fp);
```

## <mark style="color:red;">Writing a string to a text file</mark>

#### fputs()

#### int fputs(const char _str; FILE_ pfile);

First argument is a string pointer and second is a file pointer.

Will write characters from a string until it reaches a '\0' character.&#x20;

Doesnt write the null terminator character to the file can complicate reading back variable-length strings from a file that have been written by fputs().

Expecting to write a line of text that has a newline character at the end.

```c
FILE *fp;

fp = fopen("test","w+");

fputs("this is a test\n",fp); // \n character is important if you separate lines
fputs("another line here\n",fp);
fclose(fp);
```

## <mark style="color:red;">I/O functions</mark>

{% code overflow="wrap" %}
```c
#include <stdio.h>
 
 int fclose(filePtr) → close the file
 
 int feof(filePtr) → returns nonzero if the identified file has reached the end of the file and returns zero otherwise.
 
 int fflush(filePtr)  → flushes any data from internal buffers to the indicated file, returns zero on success and the value of EOF if an error occurs
 
 int fgetc(filePtr) → returns the next character in the file identified by the file pointer. the value of EOF if file an error occurs.
 
 int fgetpos(filePtr, fpos) →  gets the current file position, stores it into fpos_t (defined in stdio.h) variable pointed to by fpos.returns zero on success.
 
 char *fgets(buffer, i, filePtr) → reads characters from the indicated file until either i-1 characters are read or a newline character is read.
 
 FILE *fopen(fileName,accessMode) → open a file
 
 int fprintf (filePtr, format, arg1, arg2,...,argn) → writes spcecified args to the file identified by filePtr, according to the format specified.
 
 int fputc(c, filePtr) → writes the value of c to the file .returns c if successful and the EOF otherwise
 
 int fputs(buffer, filePtr) → writes the characters in the array pointed to by buffer to the indicated file until the terminating null character in buffer is reached
 
 int fscanf(filePtr, format,arg1,arg2,...,argn) →  reads data items from a file
 
 int fseek(filePtr,fpos) → sets the current file position for the file associated with filePtr to the value pointed to by fpos, which is of type fpos_t. returns zero on success and nonzero on failure
 
 long ftell(filePtr) → returns the relative offset in bytes of the current position in the file, or -1L on error
```
{% endcode %}

## <mark style="color:red;">Writing/reading integer to a file</mark>

```c
#include <stdio.h>

int main ()
{

    FILE *fp;
    int i=1, j=2, k=3, num;
    fp = fopen ("test","w");
    putw(i,fp);
    putw(j,fp);
    putw(k,fp);
    fclose(fp);

    fp = fopen ("test","r");

    while(getw(fp)!=EOF)
    {
        num= getw(fp);
        printf("Data in test.c file is %d \n", num);
    }
    fclose(fp);
    
    return 0;
}
```

## <mark style="color:red;">Flushing a file</mark>

<pre class="language-c"><code class="lang-c">#include &#x3C;stdio.h>
int main()
{
<strong>    FILE *fp;
</strong>    fp = fopen("test","w+");
    fflush(fp);
    
    return 0;
}
</code></pre>
