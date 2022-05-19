Exercise 20
=

Target
=

- C tackles the problem of errors by returning error codes and setting a global *errno* value that you check. This makes for complex code that simply exists to check if something you did had an error. As you write more and more C code you'll write code with the pattern:
    1. Call the function.
    2. If the return value is an error (must look that up each time too).
    3. Then cleanup all the resources created so far.
    4. and print out an error message that hopefully helps.

- This means for every function call you are potentially writing 3-4 more lines just to make sure it worked. That doesn't include the problem of cleaning up all the junk you've built to that point.
    - if you have 10 structures, 3 files, and a database connection





Code
=


Analysis
=

- line 1-2: the usual defence against accidentally including the file twice
- line 4-6: include for the functions that these macros need
- line 8: the start of #ifdef that lets you recompile your program so that all of the debug log messages are removed
- line 9: if you compile with NDEBUG defined, then *no debug* messages will remain. You can see that in this case the #define debug() is just replaced with nothing (the right side is empty)
- line 10: The matching #else for the above #ifdef
- line 11: The alternative #define debug that translates any use of debug("format", arg1, arg2) into a fprintf call to stderr. The magic here is the use of `##__VA_ARGS__` that say "put whatever they had for extra arguments (...) here". Also notice the use of `__FILE__` and `__LINE__` to get the current file:line for the debug message => very helpful
- line 12: the end of the #ifdef
- line 14: the `clean_errno` macro that's used in the others to get a safe, readable version of errno
- line 16-20: the log_err, log_warn and log_info macros for logging messages that are meant for the end user. They work like debug but can't be complied out.
- line 22: teh best macro ever, `check` will make sure the condition A is true, and if not, it logs the error M (with variable arguments for log_err), ebd then jumps to the function's error: for cleanup
- line 24: the second best macro ever, sentinel, is placed in any part of a function that shouldn't run, and if it does, it prints an error message and then jumps to the error: label. You put this in if-statements and switch-statements to catch conditions that shouldn’t happen, like the default:.
- line 26: A shorthand macro called check_mem that makes sure a pointer is valid, and if it isn’t, it reports it as an error with “Out of memory.”
- line 28: An alternative macro, check_debug, which still checks and
handles an error, but if the error is common, then it doesn’t bother
reporting it. In this one, it will use debug instead of log_err to report
the message. So when you define NDEBUG, the check still happens, and
the error jump goes off, but the message isn’t printed