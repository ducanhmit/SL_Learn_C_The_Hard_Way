
Exercise 2
= 
Using make

The Target
= 
* Learn to use make command

The Analysis
= 
* In python, you can run programs by just typing *python* and the Python Interpreter would just run them, import any libraries and things you needed.
* In C, you have to compile your source code files and manually stitch them together into a binary that can run on its own.

##  1. Using make

```console
$ make ex1
or
$ CFLAGS="-Wall" make ex1
```
Step:

    1. Does the file ex1 exist already?
    2. No. ok, is there another file that starts with ex1?
    3. Yes. it's called ex1.c. Do I know how to build .c files?
    4. Yes. I run this command `cc ex1 -o ex1` to build them
    5. I shall make you one ex1 by using cc to build it from ex1.c

You can pass modifiers to the make command or you can create these *environment variables* which will get picked up by programs you run.
```console
export CFLAGS="-Wall"
```
* -Wall tells the compiler cc to report all warnings

You can create a file call Makefile to contain all settings and variables you need to compile

```Make
CFLAGS = -Wall -g

clean:
	rm -f ex1

```
What we have here:
* first we set CFLAGS in the file so we never have to set it again, as well as adding the -g flag to get debugging
* Then we have a section call *clean* which tells make how to clean up our little project

To compile, run these commands:
```console
$ make clean
$ make ex1 
```
Output:
```console
$make clean
rm -f ex1

$ make ex1
cc -Wall -g    ex1.c   -o ex1
```