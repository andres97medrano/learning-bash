## Unix
- Unix is an OS built in late 60's by AT&T Bell Labs
- Proprietary OS (source code is private, and have to purchase)
- Written in C, it allows quicker modification, acceptance, and portability
- Default shell: `/bin/sh` (Bourne Shell)
- Unix versions:
    - HP-UX
    - AIS
    - BSD

## Linux
- OS built in '91 by Linus Torvalds
- Free and open-sourced (can download almost anywhere)
- Derived from Unix
- Can be installed on mobile phones, tablets, game consoles, etc.
- Default shell: `/bin/bash` (Bash)
- Linux versions:
    - Redhat
    - Ubuntu
    - OpenSuse
    - Solaris

## What is a script?
- A simple program that doesn't have to be compiled
- Each line is executed one after another
- Could make more complex by adding:
    - variables
    - conditionals
    - user input processing

## Core Bash Script Ingredients
1. The "shebang": `#!bin/bash`
    - Needed to tell OS what shell to use (the machine could always use a different default shell)
2. Comment lines
    - Use as internal documentation
3. White lines and other structural components
    - Make scripts readable
4. An `exit` statement
    - Use if you don't want to use the exit status of the last command in the script

See exit status of a command by running: `echo $?`. 

If a script is started as an argument to the bash shell, it doesn't need executable permission.
- E.g. `bash my-new-script-without-exe-permissions`


## Internal vs External Commands

- Internal command
    - Does not have to be loaded from disk, therefore faster
    - Use `help` to get list of these commands

- External command
    - Loaded from an executable file on disk, therefore slower

Use `type <command>` to figure out if a command is internal. 