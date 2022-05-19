Exercise 16
=

Target
=
You'll learn how to make a *struct*, point a pointer at them and use them to make sense of internal memory structures.
- construct structures from raw memory using malloc

Code
= 

Output
=
```console
$ make ex16
cc     ex16.c   -o ex16
$ ./ex16 
Joe is at memory location 0x55d5d101b2a0:
Name: Joe Alex
        Age: 32
        Height: 64
        Weight: 140
Frank is at memory location 0x55d5d101b2e0:
Name: Frank Blank
        Age: 20
        Height: 72
        Weight: 180
Name: Joe Alex
        Age: 52
        Height: 62
        Weight: 180
Name: Frank Blank
        Age: 40
        Height: 72
        Weight: 200

```

Analysis
=

**struct Person**
- a structure that has 4 elements to describe a person


**function Person_create**
- We need a way to create struct Person so we've made a function to do that
- we use malloc for memory allocate, passes the sizeof(struct Person) which calculate the total size of the struct
- we use assert to make sure that we have a valid piece of memory back from malloc


Explaining Structures
= 
- A structure in C is a collection of other data types (variables) that are stored in one block of memory but let you access each variable independently by name. They are similar to a record in one block of memory table, or a very simplistic class in an OOP language.
- The *struct Person* is a compound data type, which means you can now refer to struct Person in the same kinds of expressions you would other data types.
- This lets you pass the whole cohesive grouping to other functions, as you did with *Person_create*
- You can then access the individual parts of a *struct* by their name using x->y if you're dealing with a pointer
- If you didn't have struct, you have to figure out the size, packing, and location of pieces of memory with contents like this.