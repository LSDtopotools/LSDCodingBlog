---
layout: post
title: Debugging TopoTools
category: debugging
tags: debugging gdb valgrind
---

*DAV - Some notes mainly for my own use - I guess other folk have their favourite debugging methods*

When I first started 'debugging' I would simply insert `std::cout` statements into the code before the suspect line of code that was causing the crash or bug, to get it to print out variable values and so on. Then people started talking about this magical tool called **gdb** (The GNU debugger), which would run your program for you and then print out the most recent function calls and variables just before your code crashed. 

GDB requires you to compile your code in a certain way - you must set any optimisation flags to either `-O0` or `Og`, and you need to set the compiler to generate debugging information when it builds the executable. This is done with the `-g` flag or even `-ggdb` flag to get gdb specific deugging information. So your makefile might look something like this:

{% highlight console %}
CC = g++
CPPFLAGS = -c -0g -ggdb 

# rest of makefile as normal
{% endhighlight %}

To debug your new executable run `gdb ./MyExecutable`, note that you don't specifiy any command line arguments at this stage, as is normally done with the LSDTopoTools executable files. This is done in the next stage, so after gdb loads, type `run [PATH_TO_INPUT_DATA] [PARAMETER_FILE]`, (or whatever command line arguments and options your executable expects to receive. The program will run as normal, and output the usual stuff to the terminal. 

If your program gets to a point where it crashes, however, it will print out the error that terminated the program (often something like `Program received signal SIGSEGV, Segmentation fault`), but it will print out the very last function call and tell you the line number of the source code where the fault occurred. 

Sometimes, the output is confusing because it doesn't relate directly to one of your source code files. For example if you have a segmentation fault in the LSDTopoTools code, it may well say that the error took place in one of the TNT/Template Numerical Toolkit library headers. (like `Array2D.h` or similar). Since it's unlikely that the real cause of the bug or crash occurred in one of these library files (though not impossible), you can trace further up the call stack (i.e. list of functions that were called that lead to this crash), by typing `backtrace` or just `bt` for short. 

Hopefully, you should eventually see one of your source code files in the stack trace, and the line number in the corresponding source code file. It will also print the value of any local variables at the line number which are in scope. That is as far as it goes - it won't tell you exactly where on that line the error happened, but at least it has narrowed it down quickly!

## More detailed debugging info

If you want even more debugging information, you can type `backtrace full` or `bt full` and the debugger and it will print out *all* the variables in the current scope, as well as all the global variables. Note this is a massive terminal-filling tome of output!

As LSDTopoTools uses a lot of the Standard Template Library functions, (std::vector, std::string, etc...), these can often result in puzzling debug messages. You can get a slightly more comprehensible output by adding the `-D_GLIBCXX_DEBUG` flag to your CPPFLAGS in the makefile. If you get the gdb message `missing separate debuginfos` or similar, you might need to install a few separate linux packages before you can take advantage of this feature, see [this StackOverflow question for more information](http://stackoverflow.com/questions/10389988/missing-separate-debuginfos-use-debuginfo-install-glibc-2-12-1-47-el6-2-9-i686).

## Beyond GDB

### ddd
**ddd** is basically a GUI interface to gdb. It makes it easier to set breakpoints within the code at certain line numbers, or to track whether certain variables change. You can do all of this in plain-old terminal based gdb, but I find ddd a bit easier to use for this kind of stuff.

### Valgrind

Valgrind is a really useful tool for finding bugs that gdb/ddd would be unable to do. For example gdb is unable to tell you if you have code that can result in out of bounds array indexing, or might crash due to uninitialised variables, because it can only trace bugs that occur at run time, which may be dependent on the data/DEM or set of parameters you are currently using. Valgrind is better at finding where things *might* go wrong, even if your program isn't currently crashing. It's also used to find memory leaks - i.e. where memory is allocated for a data stucture but never deallocated. 

The main things I use Valgrind for:

1. Out of bounds array index checking
2. Finding conditional statements that are based on variables that might be uninitailised.
3. Finding memory leaks.

Valgrind is used with the command `valgrind [VALGRIND OPTIONS] ./MyProg.out [ARGUMENTS] [OPTIONS]`

The two most useful options I tend to use with valgrind are `--leak-check=yes` (to find memory leaks), and `--track-origins=yes`, which gives a bit more detail in where your program crashed/might crash. Both of these options, and valgrind in general, add a *considerable* amount of memory usage to your program, and run time is general much slower than a standard debugging run with gdb/ddd. However it's a very useful tool.




























