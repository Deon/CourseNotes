Unix and Programming Languages
========
* Not too relevant.

## Unix Shell
* `ls` - list stuff. (`-l` lists permissions)
* `dir` - stuff in dir.
* `pwd` - present working directory
* `-r` - recursively execute
* `-I` - interactive
* `-f` - force

`./myProgram < input.txt > output.txt // Changes STDIO to files.`

Linux is an OS Kernel, with functionality built into GNU on top of it.
* `cd` : home
* `cd -` : prev dir
* `cd ..` : parent
* `cd .` : no change

Shell looks for commands in the following order:

	1. Aliases
	2. Built-ins
	3. PATH commands

* `which <command>` - tells you what the command does
* `man <command>` - manual for commands
* `mkdir <namelist>` - makes directories
* `cp` - copy files into other files.
* `mv` - move
* `rm` - remove
* `cat` - prints files to shell (less - prints to shell, one page at a time)

### Permissions
* Three categories: user, group, other.
* `Xuuugggooo` - X is - if file, d if dir. permissions are ordered: rwx
* `chgrp <group name> <file/dir>` - change group
* `chmod <category> <file/dir>` - changes permissions

execute for a dir means they can cd into it; read means they can ls

* `g+r` - group can now read
* `go-rw` - group and others cannot read or write.

Can also give a three-digit octal number, each corresponds to the category.

0 = none, 7 = all permissions. add/remove as in binary. 

##Programming Languages
* 1940s - Machine 
* 1950s - FORTRAN (science), LISP, COBOL (business)
* 1960s - OO
* 1970s - BASIC
* 80s - C++
* 90s - Java


