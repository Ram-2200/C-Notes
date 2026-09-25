# C Programming — Fundamentals

> A structured reference for understanding C from syntax to runtime behavior.

## 1. What is C?

C is a compiled, statically typed, general-purpose programming language designed for efficiency, portability, and low-level memory control.

C is widely used in:

- Operating systems
- Embedded systems
- Compilers
- Networking
- Databases
- Performance-critical software
- System utilities

---

## 2. Basic Program Structure

```c
#include <stdio.h>

int main(void)
{
    printf("Hello, World!\n");
    return 0;
}
```

### Breakdown

| Component | Purpose |
|---|---|
| `#include <stdio.h>` | Includes declarations for standard input/output functions |
| `main()` | Entry point of the program |
| `printf()` | Writes formatted output |
| `return 0` | Indicates successful program termination |

### Why `int main(void)`?

Prefer:

```c
int main(void)
```

over:

```c
int main()
```

when you want to explicitly state that `main` takes no parameters.

---

# 3. Compilation Pipeline

A C source file does not directly become an executable.

```text
source.c
   │
   ▼
Preprocessing
   │
   ▼
source.i
   │
   ▼
Compilation
   │
   ▼
assembly.s
   │
   ▼
Assembly
   │
   ▼
object.o
   │
   ▼
Linking
   │
   ▼
executable
```

Example:

```bash
gcc -Wall -Wextra -std=c17 main.c -o main
```

Run:

```bash
./main
```

Useful stages:

```bash
gcc -E main.c -o main.i
gcc -S main.c -o main.s
gcc -c main.c -o main.o
gcc main.o -o main
```

---

# 4. Variables

A variable represents an object stored in memory.

```c
int age = 25;
double salary = 50000.0;
char grade = 'A';
```

Conceptually:

```text
Memory

address        value
0x1000         25       <- age
0x1004         ...
0x1008         ...
```

The exact address and layout depend on the implementation and execution environment.

---

# 5. Fundamental Data Types

Common C types include:

```c
char
short
int
long
long long

float
double
long double

_Bool
```

Do not assume every type has the same size on every platform.

Check it:

```c
#include <stdio.h>

int main(void)
{
    printf("char       : %zu\n", sizeof(char));
    printf("short      : %zu\n", sizeof(short));
    printf("int        : %zu\n", sizeof(int));
    printf("long       : %zu\n", sizeof(long));
    printf("long long  : %zu\n", sizeof(long long));
    printf("float      : %zu\n", sizeof(float));
    printf("double     : %zu\n", sizeof(double));

    return 0;
}
```

### Important

`sizeof` returns a value of type `size_t`.

Therefore:

```c
printf("%zu\n", sizeof(int));
```

is preferable to assuming it is an `int`.

---

# 6. Signed vs Unsigned Integers

```c
int x = -10;
unsigned int y = 10;
```

Signed integers can represent negative and positive values.

Unsigned integers represent only non-negative values.

Be careful when mixing signed and unsigned values:

```c
int x = -1;
unsigned int y = 1;

if (x < y)
{
    printf("x is smaller\n");
}
```

The result may surprise you because of the C integer conversion rules.

This is an important topic to study rather than relying on intuition.

---

# 7. Constants

Using `const`:

```c
const int MAX_USERS = 100;
```

The intention is that the object should not be modified through that identifier.

Incorrect:

```c
MAX_USERS = 200;
```

---

# 8. Operators

## Arithmetic

```c
+
-
*
/
%
```

Example:

```c
int a = 10;
int b = 3;

printf("%d\n", a + b);
printf("%d\n", a - b);
printf("%d\n", a * b);
printf("%d\n", a / b);
printf("%d\n", a % b);
```

Remember:

```c
10 / 3
```

produces integer division when both operands are integers.

Result:

```text
3
```

---

## Relational

```c
==
!=
>
<
>=
<=
```

## Logical

```c
&&
||
!
```

## Assignment

```c
=
+=
-=
*=
/=
%=
```

---

# 9. Type Conversion

Implicit conversion:

```c
int x = 10;
double y = x;
```

Explicit conversion:

```c
double result = (double)10 / 3;
```

Without the cast:

```c
double result = 10 / 3;
```

the division occurs as integer division first.

---

# 10. Input and Output

Output:

```c
printf("Age: %d\n", age);
```

Input:

```c
int age;

scanf("%d", &age);
```

Notice the `&`.

`scanf` needs the address where it should store the input.

This introduces one of the most important C concepts:

> **Pointers and addresses.**

---

# 11. Control Flow

## if / else

```c
if (age >= 18)
{
    printf("Adult\n");
}
else
{
    printf("Minor\n");
}
```

## switch

```c
switch (choice)
{
    case 1:
        printf("Start\n");
        break;

    case 2:
        printf("Exit\n");
        break;

    default:
        printf("Invalid choice\n");
}
```

## for loop

```c
for (int i = 0; i < 5; i++)
{
    printf("%d\n", i);
}
```

## while loop

```c
int i = 0;

while (i < 5)
{
    printf("%d\n", i);
    i++;
}
```

---

# 12. Functions

```c
int add(int a, int b)
{
    return a + b;
}
```

Usage:

```c
int result = add(10, 20);
```

A function can have:

- Return type
- Name
- Parameters
- Function body

---

# 13. Function Prototype

A declaration can appear before the function definition:

```c
int add(int a, int b);

int main(void)
{
    printf("%d\n", add(10, 20));
}

int add(int a, int b)
{
    return a + b;
}
```

The compiler uses the declaration to understand the function before encountering its definition.

---

# 14. Arrays

```c
int numbers[5] = {10, 20, 30, 40, 50};
```

Access:

```c
printf("%d\n", numbers[0]);
printf("%d\n", numbers[4]);
```

C arrays use zero-based indexing.

Valid indices:

```text
0
1
2
3
4
```

Accessing outside the array bounds results in undefined behavior.

Example:

```c
int numbers[5];

numbers[10] = 50;   // Undefined behavior
```

---

# 15. Strings

C does not have a built-in `string` type.

A string is typically represented as an array of characters terminated by `'\0'`.

```c
char name[] = "Prateek";
```

Conceptually:

```text
P r a t e e k \0
```

The null terminator is essential for standard C string functions.

Example:

```c
#include <stdio.h>

int main(void)
{
    char name[] = "Prateek";

    printf("%s\n", name);

    return 0;
}
```

---

# 16. Pointers — Preview

A pointer stores an address.

```c
int x = 10;
int *p = &x;
```

Conceptually:

```text
x
┌──────────┐
│    10    │
└──────────┘
     ▲
     │
     │ address of x
     │
┌──────────┐
│    p     │
└──────────┘
```

Dereferencing:

```c
printf("%d\n", *p);
```

`*p` accesses the object pointed to by `p`.

Pointers deserve a dedicated section because they connect C syntax to memory.

---

# 17. Undefined Behavior

One of the most important concepts in C is **undefined behavior (UB)**.

Example:

```c
int numbers[3] = {1, 2, 3};

printf("%d\n", numbers[10]);
```

The program is not guaranteed to:

- crash
- print a particular value
- produce an error
- behave consistently

The C standard places no requirements on the behavior once undefined behavior occurs.

Other examples to study:

```text
Out-of-bounds access
Use-after-free
Double free
Dereferencing invalid pointers
Signed integer overflow
Invalid shifts
Data races
```

---

# 18. Compile With Warnings

Do not develop C code with warnings ignored.

Use:

```bash
gcc -Wall -Wextra -Wpedantic -std=c17 main.c -o main
```

For debugging:

```bash
gcc -g -Wall -Wextra -Wpedantic -std=c17 main.c -o main
```

For AddressSanitizer:

```bash
gcc -g -fsanitize=address -fno-omit-frame-pointer main.c -o main
```

For UndefinedBehaviorSanitizer:

```bash
gcc -g -fsanitize=undefined main.c -o main
```

---

# 19. Practice Problems

### Beginner

1. Print numbers from 1 to 100.
2. Check whether a number is prime.
3. Reverse an integer.
4. Find the largest element in an array.
5. Count vowels in a string.
6. Implement factorial using recursion.
7. Implement Fibonacci.
8. Find the second-largest array element.

### Pointer Practice

1. Swap two integers using pointers.
2. Reverse an array using pointers.
3. Implement string length without `strlen`.
4. Implement string copy without `strcpy`.
5. Dynamically allocate an integer array.

### Memory Practice

1. Allocate memory using `malloc`.
2. Resize memory using `realloc`.
3. Properly release memory using `free`.
4. Detect and fix a memory leak.
5. Diagnose a use-after-free bug.

---

# 20. What to Learn Next

```text
Fundamentals
    ↓
Functions
    ↓
Arrays & Strings
    ↓
Pointers
    ↓
Structures
    ↓
Dynamic Memory
    ↓
Data Structures
    ↓
File Handling
    ↓
Preprocessor
    ↓
Compilation & Linking
    ↓
Memory Layout
    ↓
Processes & Threads
    ↓
Sockets
    ↓
Systems Programming
```

---

## Key Principle

Do not learn C by memorizing syntax.

For every important feature, ask:

1. What does the code mean?
2. What is stored in memory?
3. What does the compiler do?
4. What can go wrong?
5. Is the behavior defined by the C standard?
6. How would I debug it?
7. Can I implement a small project using it?

That progression is what turns C syntax knowledge into systems-level understanding.
