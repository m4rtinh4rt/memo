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

- EXP34-C. Do not dereference null pointers
- EXP12-C: Do not ignore values returned by functions

- MEM35-C: Allocate sufficient memory for an object
- MEM31-C: Free dynamically allocated memory when no longer needed

- MSC24-C: Do not use deprecated or obsolescent functions
- ERR30-C. Set errno to zero before calling a library function

- INT02-C: Understand integer conversion rules

- CON43-C: Do not allow data races in multithreaded code
- CON34-C: Declare objects shared between threads with appropriate storage durations
- CON35-C: Avoid deadlock by locking in a predefined order

### Règles pour les chaînes de caractères
- STR30-C Do not attempt to modify string literals.
- STR31-C Guarantee that storage for strings has sufficient space for character data and the null terminator.
- STR32-C Do not pass a non-null-terminated character sequence to a library function that expects a string.
- STR34-C Cast characters to unsigned char before converting to larger integer sizes.
- STR37-C Arguments to character-handling functions must be representable as an unsigned char.
- STR38-C Do not confuse narrow and wide character strings and functions.

### Recommandations pour les chaînes de caractères
- STR00-C Represent characters using an appropriate type
- STR01-C Adopt and implement a consistent plan for managing strings
- STR02-C Sanitize data passed to complex subsystems
- STR03-C Do not inadvertently truncate a string
- STR04-C Use plain char for characters in the basic character set
- STR05-C Use pointers to const when referring to string literals
- STR06-C Do not assume that strtok() leaves the parse string unchanged
- STR07-C Use the bounds-checking interfaces for string manipulation
- STR09-C Don’t assume numeric values for expressions with type plain character
- STR10-C Do not concatenate different type of string literals (char et wchar_t).
- STR11-C Do not specify the bound of a character array initialized with a string
literal

### Fonctions à nombre d’arguments variable

- EXP47-C Do not call va_arg with an argument of the incorrect type
- MSC39-C Do not call va_arg() on a va_list that has an indeterminate
value : appel préalable de va_start.

### Opérations sur les entiers : règles
- INT30-C Ensure that unsigned integer operations do not wrap
- INT31-C Ensure that integer conversions do not result in lost or
misinterpreted data
- INT32-C Ensure that operations on signed integers do not result in
overﬂow
- INT33-C Ensure that division and remainder operations do not
result in divide-by-zero errors
- INT34-C Do not shift an expression by a negative number of bits or
by greater than or equal to the number of bits that exist in
the operand
- INT35-C Use correct integer precisions
- INT36-C Converting a pointer to integer or integer to pointer

### Concurence règles

- CON43-C Do not allow data races in multithreaded code : protéger l’accès aux
variables partagées.
- CON30-C Clean up thread-specific storage : désalouer ce qui a été alloué avant
de quitter
- CON31-C Do not destroy a mutex while it is locked : risque de débloquer
anarchiquement les processus en attente.
- CON32-C Prevent data races when accessing bit-fields from multiple threads :
lors de l’accès à des champs de bits le compilateur peut lire ou écrire
des zones adjacentes
- CON33-C Avoid race conditions when using library functions : certaines
fonctions de bibliothèques ne sont pas thread safe.
- CON34-C Declare objects shared between threads with appropriate storage
durations : ne pas partager des données susceptible de ne plus
exister tq une variable locale). Privilégier des variables
globales/locales statiques.
- CON35-C Avoid deadlock by locking in a predefined order : ne pas allouer A/B
(th1) et B/A (th2), risque d’inter-blocage

### Concurence Recommandations
- CON08-C Do not assume that a group of calls to independently
atomic methods is atomic : ne pas supposer qu’un groupe
de deux opérations atomiques successives est atomique
- CON01-C Acquire and release synchronization primitives in the
same module, at the same level of abstraction : placer
dans différentes fonctions les primitives de manipulation
d’un même mutex multiplie les risques de mauvaise
utilisation.
- CON02-C Do not use volatile as a synchronization primitive :
déclarer une variable volatile ne protège pas contre les
accès concurrents
- CON04-C Join or detach threads even if their exit status is
unimportant : éviter l’équivalent des processus zombie

# TOCTOU

- Utiliser des mécanismes de verrouillage : fichiers
- Utiliser des fonctions atomiques : création de fichiers temporaires
- Ordonner les opérations : modifier les droits d’accès à un fichier
après création n’est pas atomique. Il faut modifier les droits
d’accès par defaut (umask) avant de créer le fichier.
- Ne pas utiliser de fonctions manipulant les noms de fichiers (stat) :
privilégier les fonctions manipulant des descripteurs de fichiers
(fstat).

# Client.py

```
#!/usr/bin/env python

import socket
import argparse

BUFSZ = 512

HELO = 'HEY!MMA!'
NOHELO = 'Why U no say HELO :\'('
ANSWER = 'LOL!HI!'
QUESTION = 'CANIHAS'
QUIT = 'KTHXBYE'
WIN = 'YOUWIN!'
NOP = 'NOP :\'('


def play_with_mma(host, port):
    sock = socket.socket()
    sock.connect( (host, port) )
    welcome_msg = sock.recv(BUFSZ).rstrip(b'\n').decode()
    print(welcome_msg)
    i=welcome_msg.index(' ')-1
    welcome,welcomeId=welcome_msg.split(' ');
#    welcome=welcome_msg[:welcome_msg.index(' ')-1]
#    welcomeId=welcome_msg[welcome_msg.index(' ')+1:]
#    print (" Welcome from MMA " + welcome )
#    print (" id " + welcomeId)
    if welcome == HELO:
#        print ("Welcom from MMA {0} id={1}".format(question).format(welcomeId))
        sock.send(ANSWER.encode())
        question_msg = sock.recv(BUFSZ).decode().rstrip('\n')
        print (question_msg)
        question,a,op,b_msg=question_msg.split(' ');
        print (b_msg)
        b=b_msg.rstrip('?');
        print (b)
        a=int(a)
        b=int(b)
        if op == '+':
            r = a + b
        if op == '-':
            r = a - b
        if op == '*':
            r = a * b
        if op == '/':
            r = a // b
        reponse=welcomeId + " " + str(r)
        print(reponse)
        sock.send(reponse.encode())
        quit_msg = sock.recv(BUFSZ).rstrip(b'\n').decode()
        print (quit_msg)
    else:
        sock.send(NOHELO)
        print ("Reponse send : ${0}".format(NOHELO))

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('host', help="Host to connect to", type=str)
    parser.add_argument('port', help="Port to connect to", type=int)
    args = parser.parse_args()

    play_with_mma(args.host, args.port)

if __name__ == "__main__":
    main()
```
