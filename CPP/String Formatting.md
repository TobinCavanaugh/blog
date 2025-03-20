#C #strings #CPP

---
String formatting in C++ is **RIDICULOUS**.

C++20 makes the great choice of implementing a format function like so:
```C++
#include <format>

//...
std::cout << std::format("Test {}", 123) << std::endl;
```
> Test 123

This is good, the real question, however, is why it took **35 YEARS** to add this. To demonstrate this absurdity, I implemented a similar function in C, using macros in a few hours. You can check out the actual code at [[strf]], but in the meantime, here's what the UX looks like:

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