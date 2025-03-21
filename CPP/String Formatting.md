#C #strings #CPP

---
String formatting in C++ is **RIDICULOUS**. It seems like the only way of getting what you want is to add strings together, and as a C programmer I am terrified at the thought of a plus operator doing anything but adding. I'm being ridiculous of course, but I'm touching at something real here: how do we know as programmers what adding two strings does behind the scenes?

In C, it's explicit (sometimes painfully so):
```C
char * a = "abcdef";
char * b = "xyz1234";
char c[256] = {0};
strcat_s(a, b, sizeof(c));
```
Then, in C++:
```CPP
std::string a = "abdef";
std::string b = "xyz1234";
std::string c = a + b;
```
So my question now, is where is the `c` variable string storing its data? Is it located on the stack or heap? How do I know when this cleans itself up? Maybe I'm just uneducated, but it doesn't seem apparent at all.

In C++ the way I (and others) were taught to print strings is via `std::cout` and the use of the indirection (`<<`) operator. This is braindead. The indirection operator is ugly, shadows over  

C++20 makes the great choice of implementing a format function like so:
```C++
#include <format>

//...
std::cout << std::format("Test {}", 123) << std::endl;
```
> Test 123

This is good, the real question, however, is why it took **35 YEARS** to add this. 

To demonstrate this absurdity, I implemented a similar function in C, using macros in a few hours. You can check out the actual code at [[strf]], but in the meantime, here's what the UX looks like:

```C
putf("Test ", 123);
```

This is clearly a nicer to use implementation, besides just being nicer to use, it also has a support for changing the allocator (`alloca`, `malloc`, or custom), changing formats on the fly [^1]


[^1]: Changing print formats:
```C
// i_pri is the array of float printing strings, order is
// 0: f128, 1: f64, 2: f32
// though float types are a bit more flexibile when it comes to printing
STRF_CTX.f_pri[0] = "0.2f";
putf(1.245918431);
```
> 