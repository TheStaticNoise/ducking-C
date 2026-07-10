# DUCKING C
## SOME KIND OF GUIDE IN C
# MOTIVATION
	Primary: my suffering while learning C (I'M STILL LEARNING)
# CONTENT
## Hello world!
### So, to write an basic hello world you need to write THIS

```
#include <stdio.h> // include the lib for print

int main(){ //start of the app, can return ONLY int
	printf("Hello, world!\n"); // ; obligatory
	// without \n you will get something like "Hello world! [directory]>", so don't do this
	return 0; // in main, non-obligatory return, everywhere else as far as my knowledge goes it is obligatory
}
```

and yeah, this '//' means comments
so, it looks easy, but it's the basics, when we will get to character manipulation, pointers or etc we will be suffering, so, there's all posible mistakes you can make
1. Do not include <stdio.h>
2. forget () in main
3. forget ;
4. do not write return
5. return something other than an int
6. do not open/close the brackets
knowing this, go and poke around doing those, observing what errors you get, but wait...
Do you have a compiler? If no, then go on the internet and search for it, the downloads for mingw compiler :
here >> https://www.mingw-w64.org/downloads/ << or if you're on linux, use your package manager to install base developing tools (base-devel)
now install, set it up with your IDE(zed, vs code, whatever)
And what about variables you probably will ask (oh, here the hell begins),
Strings are actually charsets, and not just charsets, charsets from hell, look
char x[] = "Hello, world!";
what you can do wrong here?
1. do not identify that this is char
2. forget [], and then its wrong, you can do either : 
char *a = "bob"; // read only, Undefined Behavior on trying to edit
//or
char a[] = "bob"; // editable, still fixed size 
3. fyi, [] is the array declaration, you could think that the array size would be 13 because it has 13 characters?...
COMPLETELY WRONG, you need to know, that you should add to every char array size 1 because of \0, the fucking null terminator(this is how it's called)
this mf shows that the text had ended to a function, because normal practice for traversing a text in C is while (*text), it stops when encountering null terminator 
if you determine one time and don't change the array, just use 'char x[] = "hello"', the compiler will do it itself, but,
if you do operations with strings during runtime, then you need to know, you need a library called <string.h> ofc you can write functions yourself, but save you some time

you can do this with []
int a[] = {0, 1, 2, 3, 4, 5};
declare an array of 6 ints (on 32+ bit systems 32 bit)
char a[] = "bob";
declare an array of 4 characters (4 bytes) cuz of 3 + 1 for null terminator

# Variables

Variables in C are simple, their syntax is

type name (non obligatory assigment);

so you can do
1. int integer;
2. int answer = 42;

there are 5 day to day types

char
int
float
double
void (yeah, empty basically, you cannot make a void variable, but YOU can do a void pointer, which is just a pointer to memory without expected type)

you can make an array by using

type name[size];

or if you have your data at compilation time

type name[] = {data1, data2, data3, ...};

## MALLOC HELL
So, we got to allocated memory (malloc -> memory alloc), congrats, so, if you want to allocate memory, you need to include the <stdlib.h>, then you need to do

```
#include <stdio.h> // just for output, you'll se why
#include <stdlib.h> // what we need

int main() {
	int *x = malloc(sizeof(int)); // the needed malloc, it allocates memory, so you can use it later, sizeof is a standard function, which is so fucking usefull
	//and... the worst part, malloc can basically say "fuck you!" if there is no memory, so, you need to check
	if (x == NULL) {
		printf("Congrats, malloc said \"fuck off\" \n"); // to write " in text you need to do \" btw
		return 1;
	}
	//No need for else
	*x = 5;
	printf("x set to: %d\n", *x); // marker for a digit is %d, this is what you need to know until you will learn the actual thing
	free(x);
	return 0;
}
```

So, if you don't know (which you most probably do), free will free the variable from memory and let the space be taken again (which is highly recomended), it works like this, this kind of bug when you don't free the memory is called memory leak, even tho the OS will take the memory back after the app exited, but during runtime, you have no GC, if you don't free the memory, congrads, you wasted the size of allocation, imagine

for (int a = 0; a < 10; a++) {
q = malloc(56);
// we use q there
// no free!
}
q = malloc(sizeof(int));

We wasted 560 bytes, aka nearly a half KB of ram, malloc still thinks the memory is in use and while the app is running, 
you occupy 560 bytes more than needed, allocs in loops are risky, but its not the only scenario for a memory leak, remember, 
always free your memory, its not C++ to manage your memory for you.

# IF introduction

if (expression) {//here the code goes}

as basic operations you have equals(==), not equals (!=), lower(<), bigger(>), or, if you have <stdbool.h>, you can have true and false

and, here's what you can do wrong in the code from before
1. don't use * (get the address not the variable)
2. don't free the variable (memory leak)

so, we got an example for int, now we need to know 3 main variables and strings
1. char // one char, just one
2. int // basic integer (1, 2 etc)
3. float // example. 1.01
4. char[] //string

and 2 additional
1. double (more precise float)
2. void (basically nothing)

array, struct and etc are not data types, they are data structures.

## hell of the conditions, if, else, switch, and cases

this shit is the basics of programming, it checks if the condition is true, or, in case of else, if the if's condition is false,

here is a code example for if else

```
int main() {
	int a = 5;
	int b = 6;
	int c = 7;
	if (a == 5) { //true, because == means - equals to
		printf("A equals to 5\n");
	}
	if (b != 6) { //false, because != means - not equals to
		printf("B not equals to 6\n");
	} else if (b == 6) { // here we use else, and if after it
		printf("B equals to 6");
	}
	if (c < 7) { //another condition, means smaller than
		printf("C is smaller than 7\n");
	} else if (c > 7) { //the same but means bigger than
		printf("C is bigger than 7\n");
	} else { //activates if both return false
		printf("C equals to 7\n");
	}
}
```

What can you do wrong?
1. do not put the condition in ()
2. do not put the code in {}
3. write = instead of ==
4. write nonsense that will never be triggered
5. try to compare a string like this ``` if (a == "ABC")``` (p.s, it won't work, you need strcmp from string.h, which we will cover later)

and now for switch (I HATE IT, BUT IT'S BETTER THAN IF ELSE IN IT'S PART)
for example a simple calculator

```
void clear_input_buffer() { //copied from google, I hate C, but this will safe us from accidental \n in buffer
    int c; // FOR NUMBERS ONLY
    while ((c = getchar()) != '\n' && c != EOF); // while, which we will cover later, && - and, and if c is not End-Of-File symbol
}

int main() {
	//gonna leave here a lolz for writer's (aka mine) emotional support, LOLZ
	int a, b, c; //yeah, grouped declaration, remember this
	char op;
	printf("enter first number: \n"); //not stacked for readability, don't complain about that I could made it more compact
	scanf("%d", &a); // we didn't cover it, it's part of I/O, but we will get to it
	printf("\nenter second number: \n");
	scanf("%d", &b);
	clear_input_buffer(); //this is how we call a function
	printf("\nenter the operation: \n");
	scanf("%c", &op); // char - %c, for output, and input
	switch (op) {
		case '+':
			c = a + b;
			break; // don't forget it, you will hate forgetting it
		case '-':
			c = a - b;
			break; // explained better in what you can do wrong section
		case '*':
			c = a * b;
			break;
		case '/':
			c = a / b;
			break;
		default: //caused if none of the cases caused break;
			printf("No operation like this found\n");
			return -1; //improvised error code
	}
	printf("%d %c %d = %d", a, op, b, c); //basically will show what calculation we did with the result
	return 0;
}
```

what can you do wrong?
1. FORGET BREAK (NEVER)
2. forget to put the value in ()
3. do not have a fail-safe as default
4. do not put the condition

# POINTERS (formal education, even tho you already saw them)

what are pointers? pointers show a place in ram, where you can find data, and here comes 1 question
What is a segmentation fault? Seg fault is when an app tries to access memory it has no permission to access, OS keeps tabs on memory, also, there is 2 kinds, cpu level (illegal writes,
unpaged memory, also called page fault) and kernel one (when you access memory you don't own) 
what is a pointer?

actually "type* name;" isn't auto allocation, but more like an automatic hint to compiler how to interpret the variable, you need to allocate or point to the memory yourself
for pointers -> * - dereference (get value), & - get memory

# DEBUG THIS!
edit this code until it runs (If you use AI for that, please, ask it how it did, what error were there, how it fixed them, and so on)
(P.S return 'Q' is completely valid cuz of automatic promotion, which we will talk about later)

```
int main( {
    printf("Hello world!\n'');
    int* q = malloc(sizeof(int));
    *q = 5;
    int answer = 42;
    q = &answer;
    printf("%d\n", *q);
    switch (answer):
        case (42) {
            printf("Not 42!\n");
        }
        case (32) {
            printf("Ducking C!\n");
            break;
        }
        default:
            printf("XD\n");
    return 'Q';
}
```

```
compile with: [compiler] -Wall -Wextra -g [input file] -o [output file]
example:
gcc -Wall -Wextra -g Broken.c -o Broken.[o/out/exe]
./broken.[o/out/exe]
```

What every flag does:
-Wall : Warning All, all standard warning enabled
-Wextra : extra warnings
-g : embed into the file debug symbol/names
-o : mark the output file
while input file can go anywhere, the output file should always be after -o
If compiled: cool, but check for logical errors or memory ones, if doesn't, fix the ones that the compiler was complaining the most
repeat, if you want, use valgrind and gdb, valgrind is used for memory leaks, while gdb for crashes, warnings are mostly fine,
its just the compiler complaining about things, also, always test the app before production, please, and never let it work with real data if
it has writing capabilities or deletion ones

## So, about Stack and Heap

What is stack and heap you may ask, and there's the answer :
Stack is a place given to you by your OS (on windows 2Mb by std, on linux from 8 to 16Mb)
Its fast cuz no allocations, but its limited
Heap on the other end is a place where you can throw any shit and it will be okay, but, you need to allocate it, you can write your own allocator
or use the <stdlib.h> one, which is honestly the best choice for most apps

Stack - last in first out (imagine plates)
Heap - cool, but if not marked free then the allocator doesn't know it can use the memory and that you don't need it
C++ introduced RAII, which we won't talk about much, its a C guide, not C++, all you need to know is that it means: Resource Acquisition Is Initialization
It basically means that when you create something you initialize it, it has a constructor and a deconstructor, which is called automatically (Damn QoL)

## "Advanced" memory allocations

So, we have another 2 functions

(footprints)
(P.S. size_t comes from <stddef.h>, its the same size as void*, its word size variable, which means it has as many bits as the processor can handle
in one batch)
```
void *calloc(size_t __nmemb, size_t __size) - allocate zero'ed out memory (nmemb is how many and size how big chunks, good for arrays and shit)
void *realloc(void *__ptr, size_t __size) - reallocate memory (either extends in its list the size or allocates a new chunk and copies data, yup)
```
also, realloc can return null, but the previous pointer still works, so, store the reallocated pointer in temp variable and check for null
You'll need both of these someday

example:
```
#include <stddef.h> // will either way be pulled in automatically, so why not include?
#include <stdio.h> // for the sake of output
#include <stdlib.h> // our allocators live here

int main(void) { // did you know, the proper way to not take any arguments is to put in arguments just void, but nowadays compilers allow you to just not write anything
    void *temp, *temp2;

    temp = calloc(5, sizeof(int)); // we now got a zeroed out array 5 integers wide
    if (!temp) { // NULL check
        printf("Calloc failed\n");
        return -1;
    }
    temp2 = realloc(temp, sizeof(int) * 10); // resize to 10 ints (first 5 zeroed out all other ones are filled with garbage)
    if (!temp2) {
        printf("Realloc failed\n");
        return -1;
    }
    temp = temp2; // change to our new pointer (the previous one is either freed or points to the same memory, but lets assume it moved, always assume that)
    free(temp);
    return 0;
}
```
