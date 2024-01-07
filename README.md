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

# Client.py

python
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
