# Mémo

## FLag

```
CFLAGS=-Wall -Wextra -ansi -pedantic -Werror -pie -fPIE -DFORTIFY_SOURCE=2 -O1 -fstack-protector
LDFLAGS=-Wl,-z,relro -Wl,-z,now
```

## Define

```
#define _XOPEN_SOURCE 500
```

## Rules

EXP34-C. Do not dereference null pointers
EXP12-C: Do not ignore values returned by functions

MEM35-C: Allocate sufficient memory for an object
MEM31-C: Free dynamically allocated memory when no longer needed

MSC24-C: Do not use deprecated or obsolescent functions
ERR30-C. Set errno to zero before calling a library function

INT02-C: Understand integer conversion rules

CON43-C: Do not allow data races in multithreaded code
CON34-C: Declare objects shared between threads with appropriate storage durations
CON35-C: Avoid deadlock by locking in a predefined order
