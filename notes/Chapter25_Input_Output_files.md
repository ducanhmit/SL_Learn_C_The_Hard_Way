Exercise 24
=


Target
=

- You’ve been using printf to print things, and that’s great and all, but you need more. In this exercise, you’ll be using the functions fscanf and fgets to build information about a person in a structure. After this simple introduction about reading input, you’ll get a full list of the functions that C has for I/O. Some of these you’ve already seen and used, so this will be another memorization exercise.

Code
=
```c
#include <stdio.h>
#include "dbg.h"

#define MAX_DATA 100

typedef enum EyeColor {
    BLUE_EYES, GREEN_EYES, BROWN_EYES,
    BLACK_EYES, OTHER_EYES
} EyeColor;

const char *EYE_COLOR_NAMES[] = {
    "Blue", "Green", "Brown", "Black", "Other"
};

typedef struct Person {
    int age;
    char first_name[MAX_DATA];
    char last_name[MAX_DATA];
    EyeColor eyes;
    float income;
} Person;

int main(int argc, char *argv[])
{
    Person you = {.age = 0};
    int i = 0;
    char *in = NULL;

    printf("What's your First name? ");
    in = fgets(you.first_name, MAX_DATA - 1, stdin);
    check(in != NULL, "Failed to read first name,");

    printf("What's your Last Name? ");
    in = fgets(you.last_name, MAX_DATA - 1, stdin);
    check(in != NULL, "Failed to read last name.");

    printf("How old are you? ");
    int rc = fscanf(stdin, "%d", &you.age);
    check(rc > 0, "You have to enter a number.");

    printf("What color are your eyes:\n");
    for (i = 0; i <= OTHER_EYES; i++) {
        printf("%d) %s\n", i + 1, EYE_COLOR_NAMES[i]);
    }
    printf("> ");

    int eyes = -1;
    rc = fscanf(stdin, "%d", &eyes);
    check(rc > 0, "You have to enter a number.");

    you.eyes = eyes - 1;
    check(you.eyes <= OTHER_EYES 
        && you.eyes >= 0, "Do it right, that's not an option.");

    printf("How much do you make an hour? ");
    rc = fscanf(stdin, "%f", &you.income);
    check(rc > 0, "Enter a floating point number.");

    printf("----- RESULTS -----\n");

    printf("First Name: %s", you.first_name);
    printf("Last Name: %s", you.last_name);
    printf("Age: %d\n", you.age);
    printf("Eyes: %s\n", EYE_COLOR_NAMES[you.eyes]);
    printf("Income: %f\n", you.income);
    
    return 0;
error:
    return -1;
}
```

Analysis
=

This program is deceptively simple, and introduces a function called fscanf, which is the file scanf. The scanf family of functions are the inverse of the printf versions. Where printf printed out data based on a format, scanf reads (or scans) input based on a format. There’s nothing original in the beginning of the file, so here’s what the main is doing in the program:
- **ex24.c:24-28** Sets up some variables we’ll need.
- **ex24.c:30-32** Gets your first name using the fgets function, which reads a string from the input (in this case stdin), but makes sure it doesn’t overflow the given buffer.
- **ex24.c:34-36** Same thing for you.last_name, again using fgets.
- **ex24.c:38-39** Uses fscanf to read an integer from stdin and put it into you.age You can see that the same format string is used as printf to print an integer. You should also see that you have to give the address of you.age so that fscanf has a pointer to it and can modify it. This is a good example of using a pointer to a piece of data as an out parameter.
- **ex24.c:41-45** Prints out all of the options available for eye color, with a matching number that works with the EyeColor enum above.
- **ex24.c:47-50** Using fscanf again, gets a number for the you.eyes, but make sure the input is valid. This is important because someone can enter a value outside the EYE_COLOR_ NAMES array and cause a segmentation fault.
- **ex24.c:52-53** Gets how much you make as a float for the you.income.
- **ex24.c:55-61** Prints everything out so you can see if you have it right. Notice that EYE_COLOR_NAMES is used to print out what the EyeColor enum is actually called.


Output
=

```console
$ make ex24
cc -Wall -g -DNDEBUG    ex24.c   -o ex24
$ ./ex24
What's your First name? Luu
What's your Last Name? Duc Anh
How old are you? 27
What color are your eyes:
1) Blue
2) Green
3) Brown
4) Black
5) Other
> 4
How much do you make an hour? 5
----- RESULTS -----
First Name: Luu
Last Name: Duc Anh
Age: 27
Eyes: Black
Income: 5.000000
```

How to Break It
=

- This is all fine and good, but the really important part of this exercise is how scanf actually sucks.
It’s fine for a simple conversion of numbers, but fails for strings because it’s difficult to tell scanf
how big a buffer is before you read it. There’s also a problem with the function gets (not fgets,
the non-f version), which we avoided. That function has no idea how big the input buffer is at all and
will just trash your program.
- To demonstrate the problems with fscanf and strings, change the lines that use fgets so they are
fscanf(stdin, "%50s", you.first_name), and then try to use it again. Notice it seems to
read too much and then eat your enter key? This doesn’t do what you think it does, and rather than
deal with weird scanf issues, you should just use fgets.
- Next, change the fgets to use gets, then run your debugger on ex24. Do this inside:
"run << /dev/urandom"
- This feeds random garbage into your program. This is called fuzzing your program, and it’s a good
way to find input bugs. In this case, you’re feeding garbage from the /dev/urandom file (device),
and then watching it crash. In some platforms, you may have to do this a few times, or even adjust the
MAX_DATA define so it’s small enough.
- The gets function is so bad that some platforms actually warn you when the program runs that
you’re using gets. You should never use this function, ever.
- Finally, take the input for you.eyes and remove the check that the number is within the right range.
Then, feed it bad numbers like -1 or 1000. Do this under the debugger so you can see what happ