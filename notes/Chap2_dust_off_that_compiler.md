Firstly, create your first simple program in C.

```c
#include <stdio.h>

int main(int argc, char **argv) 
{
    puts("Hello world!");

    return 0;
}
```

To compile your program, run:

```console
$ make ex1
cc ex1.c -o ex1
```

Output
```console
$ ./ex1
Hello world!
```

How to break it
- Use **-Wall** option to show all Warnings from C compiler
```console
$ CFLAGS="-Wall" make ex1
cc -Wall    ex1.c   -o ex1
```