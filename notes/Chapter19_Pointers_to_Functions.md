Exercise 18
=

Code
=


Output
=

```console
$ make ex18
cc -Wall -g    ex18.c   -o ex18
$ ./ex18 4 1 7 3 2 0 8
0 1 2 3 4 7 8 
8 7 4 3 2 1 0 
3 4 2 7 1 0 8 
```



Analysis
=
- Functions in C are actually just pointers to a spot in the program where some code exists. You can point a pointer to a function like you've been creating pointers to structs, strings and arrays.
- The main use for this is to pass *callbacks* to other functions, or to simulate classes and objects.
- The format of a funtion pointer goes like this:
```c
int (*POINTER_NAME) (int a, int b)
```

- A good way to remember how to write a pointer to a function is to do this:
    1. write a normal function declaration:
    ```c
    int callme(int a, int b)
    ```
    2. Wrap function name with a pointer syntax:
   ```c
   int (*callme)(int a, int b)
   ```