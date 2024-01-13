# What's that?

Instruction to build _target_ from _dependencies_, using _commands_

```
target:   dependencies ...
          commands
          ...
```
---
# Example
Generating an _.html_ file from a Markdown file.

```
hello.html: hello.md
	markdown hello.md > hello.html
```
---
# Example
Adding a _clean_ target

```
hello.html: hello.md
	markdown hello.md > hello.html

.PHONY: clean

clean:
	-rm hello.html
```
---
# Targets can be anything!
```
hello.html: hello.md
	markdown hello.md > hello.html

.PHONY: clean browse

clean:
	-rm hello.html

browse:
	xdg-open hello.html
```
---
# Building C/C++ programs

Building a set of C files.

```
helloworld:
	gcc -ohelloworld helloworld.c main.c

.PHONY: clean

clean:
	-rm helloworld *.o
```
---
# A more complete example
```
all: helloworld

.PHONY: clean

clean:
	-rm helloworld.o main.o

helloworld.o: helloworld.c
	gcc -c -ohelloworld.o helloworld.c

main.o: main.c
	gcc -c -omain.o main.c

helloworld: main.o helloworld.o
	gcc -ohelloworld main.o helloworld.o
```
---
# Using implicit rules
```
...
all: helloworld

.PHONY: clean

FILES=helloworld.o main.o

clean:
	-rm helloworld.o main.o helloworld

helloworld: $(FILES)
	gcc -ohelloworld $(FILES)
```
---
# GNU Make implicit rules
https://www.gnu.org/software/make/manual/make.html#Catalogue-of-Rules
---
# Pattern rules
```
all: hello.html bye.html

TARGET=hello.md bye.md

.PHONY: clean
clean:
	-rm *.html

hello.html: hello.md
bye.html:   bye.md

%.html: %.md
	markdown $< > $@
```
note: https://www.gnu.org/software/make/manual/make.html#Pattern-Rules