# Symbolic links

Symbolic links points to another FILE.
![[Symbolic Links]]
## In practice

### Creating a symbolic link

```
$ echo "Hello world" > some.file
$ ln -s some.file some.file.symlink 
$ ls -l
total 4
-rw-r--r-- 1 neumann neumann 12 Jan  4 14:19 some.file
lrwxrwxrwx 1 neumann neumann  9 Jan  4 14:19 some.file.symlink -> some.file
$ cat some.file.symlink
Hello world
$ cat $(readlink some.file.symlink)
Hello world
```
### Deleting the file

```
$ rm some.file
$ ls -l
total 0
lrwxrwxrwx 1 neumann neumann 9 Jan  4 14:19 some.file.symlink -> some.file
$ cat some.file.symlink
cat: some.file: No such file or directory
$ cat $(readlink some.file.symlink)
cat: some.file: No such file or directory
$ echo "Hello world" > some.file
$ cat some.file.symlink
Hello world
```
# Hard links

Hard links point the same data as another file.
![[Hard Links]]
## In practice

## Creating a hard link

```
$ echo "Hello world" > some.file
$ ln some.file some.file.link
$ ln -l
-rw-r--r-- 2 neumann neumann 12 Jan  4 14:24 some.file
-rw-r--r-- 2 neumann neumann 12 Jan  4 14:24 some.file.link
```
### Deleting the file

```
$ rm some.file
$ cat some.file.link
Hello world
```

# More information

https://medium.com/@307/hard-links-and-symbolic-links-a-comparison-7f2b56864cdd