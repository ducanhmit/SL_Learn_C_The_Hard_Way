Exercise 9
= 
Arrays And Strings

Target
=
Know the similarity between arrays and strings

Code
= 
```c
#include <stdio.h>

int main (int argc, char *argv[])
{
    int numbers[4] = {0};
    char name[4] = {'a'};

    // first, print them out raw
    printf("numbers: %d %d %d %d\n",
            numbers[0], numbers[1],
            numbers[2], numbers[3]);
    
    printf("name each: %c %c %c %c\n",
            name[0], name[1], name[2], name[3]);

    printf("name: %s\n", name);

    // setup the numbers
    numbers[0] = 1;
    numbers[1] = 2;
    numbers[2] = 3;
    numbers[3] = 4;

    // setup the name
    name[0] = 'a';
    name[1] = 'n';
    name[2] = 'h';
    name[3] = '\0';

    // then print them out initialized
    printf("numbers: %d %d %d %d\n",
           numbers[0], numbers[1],
           numbers[2], numbers[3]);

    printf("name each: %c %c %c %c\n",
           name[0], name[1], name[2], name[3]);

    // print the name like a string
    printf("name: %s\n", name);
    
    // another way to use name
    char *another = "Anh";

    printf("anoither: %s\n", another);

    printf("another each: %c %c %c %c\n",
            another[0], another[1], another[2], another[3]);

    return 0;
}

```

Output
=
```console
$ make ex9
cc     ex9.c   -o ex9
$ ./ex9 
numbers: 0 0 0 0
name each: a   
name: a
numbers: 1 2 3 4
name each: a n h 
name: anh
anoither: Anh
another each: A n h 
```


Analysis
= 

- C stores its strings simply as a an arrays of bytes, terninated with the '\0' (null) byte.
- You don't have to give all 4 elements of the arrays to initialize them. If you set just one element, it'll fill the rest in with 0.
- When each element of numbers is printed they all come out as 0
- When each element of name is printed, only the first element 'a' shows up because the '\0' character is special and won't display.
- Syntax char *another = "name" is common in C to make string


Break it
= 
- The source of almost bugs in C come from forgetting to have enough space, or forgetting to put a '\0' at the end of a string 
