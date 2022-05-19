Exercise 25
= 


Target
=

- In C, you can create your own versions of functions like printf and scanf by creating a variable argument function, or vararg function. These functions use the header stdarg.h, and with them, you can create nicer interfaces to your library. They are handy for certain types of builder functions, formatting functions, and anything that takes variable arguments.
- Understanding vararg functions is not essential to creating C programs. I think I’ve used it maybe 20
times in my code in all of the years I’ve been programming. However, knowing how a vararg function works will help you debug the programs you use and gives you a better understanding of the computer.



Code
================






Analysis
=

- This program is similar to the previous exercise, except I have written my own scanf function to
handle strings the way I want. The main function should be clear to you, as well as the two functions
read_string and read_int, since they do nothing new.
- The varargs function is called read_scan, and it does the same thing that scanf is doing using the va_list data structure and supporting macros and functions. Here’s how;
  - I set as the last parameter of the function the keyword ... to indicate to C that this function will take any number of arguments after the fmt argument. I could put many other arguments before this, but I can’t put any more after this.
  - After setting up some variables, I create a va_list variable and initialize it with va_start. This configures the gear in stdarg.h that handles variable arguments.