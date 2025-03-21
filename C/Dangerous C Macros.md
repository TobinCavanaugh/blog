This concept is from Brian Kernighan's talk: [Elements of Programming Style](https://youtu.be/8SUkrR7ZfTA?t=1292)
## Problem:
```C
#define DoThing(a) ({a;a;a;});

int main(){
	int i = 0;
	DoThing(i++);
	// Expected: 1
	// Real: 3
	printf("%d", i);
}
```

This macro demonstrates that the interface of a macro can look nice and safe (like a function), but in reality behave in a distinctly non-function-like manner.

This example results in:
```C
int i = 0;
DoThing(i++);
// Expands to:
({ i++; i++; i++; })
```
## Solution (GCC):
```C
#define DoThing(a) ({int x = a; x; x; x;})

int main(){
	int i = 0;
	DoThing(i++);
	// Expected: 1
	// Real: 1
	printf("%d", i);
}
```

## Solution (Standard C):
```C
#define DoThing(a) do { int x = a; x; x; x; } while (0)

int main(){
	int i = 0;
	DoThing(i++);
	// Expected: 1
	// Real: 1
	printf("%d", i);
}
```

##### Upsides:
- Solves the problem
- Doesn't impact user negatively
- Enforces some type (unless `__auto_type` or `typeof` is used)

##### Downsides:
- Enforces some type
- May require statement expressions, which are GNU C