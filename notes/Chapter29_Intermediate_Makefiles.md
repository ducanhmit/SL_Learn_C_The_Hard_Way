Target
=

- Create a skeleton project directory to use in building your C programs later. This skeleton directory will be used for the rest of the book. The purpose of this structure is to make it easy to build medium-sized programs without having to resort to configure tools.
- In this exercise, I’ll cover just the Makefile so you can understand it.


The Basic Project Structure
=
```console
$ mkdir c-skeleton
$ cd c-skeleton/
$ touch LICENSE README.md Makefile
$ mkdir bin src tests
$ cp dbg.h src/ # this is from ex20
$ ls -l
total 12
drwxrwxr-x 2 ducanh ducanh 4096 May 11 16:42 bin
-rw-rw-r-- 1 ducanh ducanh    0 May 11 16:42 LICENSE
-rw-rw-r-- 1 ducanh ducanh    0 May 11 16:42 Makefile
-rw-rw-r-- 1 ducanh ducanh    0 May 11 16:42 README.md
drwxrwxr-x 2 ducanh ducanh 4096 May 11 16:42 src
drwxrwxr-x 2 ducanh ducanh 4096 May 11 16:42 tests
$ ls -l src
total 4
-rw-rw-r-- 1 ducanh ducanh 978 May 11 16:44 dbg.h
```

Breakdown
- **README.md** Basic instructions for using your project go here. It ends in .md so that it will be
interpreted as markdown
- **Makefile** The main build file for the project
- **bin/** Where programs that users can run go. This is usually empty, and the Makefile will create it if it’s not there.
- **build/** Where libraries and other build artifacts go. Also empty, and the Makefile will create it if it’s not there.
- **src/** Where the source code goes, usually .c and .h files.
- **tests/** Where automated tests go.
- **src/dbg.h** I copied the dbg.h from Exercise 19 into src/ for later


Makefile
=
- The first thing I’ll cover is the Makefile, because from that you can understand how everything else works. The Makefile in this exercise is much more detailed than ones you’ve used so far, so I’ll break it down after you type it in:

```makefile
CFLAGS=-g -O2 -Wall -Wextra -Isrc -rdynamic -DNDEBUG $(OPTFLAGS)
```


1. The Header
- This Makefile is designed to build a library reliably on almost any platform using special features of GNU make. We’ll be working on this library later, so I’ll break down each part in sections, starting with the header.

- **Makefile:1** These are the usual CFLAGS that you set in all of your projects, along with a few others that may be needed to build libraries. You may need to adjust these for different platforms. Notice the OPTFLAGS variable at the end that lets people augment the build options as needed.
- **Makefile:2** These options are used when linking a library. Someone else can then augment the
linking options using the OPTLIBS variable
- **Makefile:3** This code sets an optional variable called PREFIX that will only have this value if the
person running the Makefile didn’t already give a PREFIX setting. That’s what the ?= does
- **Makefile:5** This fancy line of awesomeness dynamically creates the SOURCES variable by doing a wildcard search for all *.c files in the src/ directory. You have to give both src/**/*.c and src/*.c so that GNU make will include the files in src and the files below it.
- **Makefile:6** Once you have the list of source files, you can then use the patsubst to take the SOURCES list of *.c files and make a new list of all the object files. You do this by telling patsubst to change all %.c extensions to %.o, and then those extensions are assigned to
OBJECTS.
- **Makefile:8** We’re using the wildcard again to find all of the test source files for the unit tests.
These are separate from the library’s source files.
- **Makefile:9** Then, we’re using the same patsubst trick to dynamically get all the TEST targets. In this case, I’m stripping away the .c extension so that a full program will be made with the same name. Previously, I had replaced the .c with {.o} so an object file is created



2. The Target Build
=

Continuing with the breakdown of the Makefile, I’m actually building the object files and targets:
- **Makefile:15** Remember that the first target is what make runs by default when no target is given. In this, it’s called all: and it gives $(TARGET) tests as the targets to build. Look up at the TARGET variable and you see that’s the library, so all: will first build the library. The tests target is further down in the Makefile and builds the unit tests.
- **Makefile:17** Here’s another target for making “developer builds” that introduces a technique for changing options for just one target. If I do a “dev build,” I want the CFLAGS to include options like -Wextra that are useful for finding bugs. If you place them on the target line as options like this, then give another line that says the original target (in this case all), then it will change the options you set. I use this for setting different flags on different platforms that need it.
- **Makefile:20** This builds the TARGET library, whatever that is. It also uses the same trick from line 15, giving a target with just options and ways to alter them for this run. In this case, I’m adding -fPIC just for the library build, using the += syntax to add it on.
- **Makefile:20** Now we see the real target, where I say first make the build directory, and then compile all of the OBJECTS.
- **Makefile:21** This runs the ar command that actually makes the TARGET. The syntax $@
$(OBJECTS) is a way of saying, “put the target for this Makefile source here and all the
OBJECTS after that.” In this case, the $@ maps back to the $(TARGET) on line 19, which maps
to build/libYOUR_LIBRARY.a. It seems like a lot to keep track of in this indirection, and it
can be, but once you get it working, you just change TARGET at the top and build a whole new
library.
- **Makefile:22** Finally, to make the library, you run ranlib on the TARGET and it’s built.
- **Makefile:23-24** This just makes the build/ or bin/ directories if they don’t exist. This is then referenced from line 21 when it gives the build target to make sure the build/ directory is made.


3. The Unit Tests
=

C is different from other languages because it’s easier to create one tiny little program for each thing you’re testing. Some testing frameworks try to emulate the module concept other languages have and do dynamic loading, but this doesn’t work well in C. It’s also unnecessary, because you can just make a single program that’s run for each test instead.



4. The Cleaner
=


5. The Install
= 

After that, I’ll need a way to install the project, and for a Makefile that’s building a library, I just need to put something in the common PREFIX directory, usually /usr/local/lib.
- **Makefile:45** This makes install: depend on the all: target, so that when you run make
install, it will be sure to build everything.
- **Makefile:46** I then use the program install to create the target lib directory if it doesn’t exist. In this case, I’m trying to make the install as flexible as possible by using two variables that are conventions for installers. DESTDIR is handed to make by installers, which do their builds in secure or odd locations, to build packages. PREFIX is used when people want the project to be installed in someplace other than /usr/local.
- **Makefile:47** After that, I’m just using install to actually install the library where it needs to go.

The purpose of the install program is to make sure things have the right permissions set. When
you run make install, you usually have to do it as the root user, so the typical build process is
make && sudo make install

