### Argument
- Anything that can be used after a command.
- e.g. `ls -l /etc` has 2 arguments

### Option
- An *argument* that was specifically developed to change the command behavior.
- e.g. `-l` is an option. It is part of the `ls` command. 
- Run `bash -x` to get some debug line-by-line of what your script is running
    - e.g. `bash -x yourscript`

### Parameter
- A name that is defined in the script to which a specific value is granted.
- Not dealed with that much in scripting

### Variable
- A label stored in memory that contains a specific value.
- Best practice: Use CAPITAL letters for variables. It makes the script easier to read as the file gets bigger.

---

### Three ways to define a variable

1. Static
    - VAR=value
2. As argument
    - VAR1=$1
3. Interactively
    - Using `read`

### `export`

If you want variables to be available outside of the script, or in subshells, you would use `export`. 
- e.g. `export COLOR=red`

There is no way to make variables available in a parent shell.

What if we want certain variables available at the start of each shell?

We have the `/etc/profile` which is processed when opening a login shell.
- All variables defined here are available in all subshells for that specific user
- User-specific version can be used in `~/.bash_profile`

We also have `/etc/bashrc` which is processed when opening a subshell.
- Variables defined here are included in subshells from that point on
- User-specific versions can be used in `~/.bashrc`

### Using `read` to define variable

- e.g. 
```bash
echo What is your name? 
read USER_NAME
```
- it pauses the script until a user presses the Enter key
- can also do `read -a INPUT_ARRAY` to get an array of strings
- when we run a script from our terminal, they run in a subprocess

### Using `source`
- use the `source` command or the `.` command to source scripts
- by using sourcing, the contents of one script, can be used in another script
- common method to separate static script code from dynamic code
    - dynamic scipts often consist of variables and functions
- DO NOT use `exit` at the end of a script that needs to be sourced

- e.g. `/root/sourceme` exposes the variable `COLOR` 
```bash
COLOR=yellow
```
In the file we want to obtain the `COLOR` variable, we would do:
```bash
. /root/sourceme

echo $COLOR
```
and it would print: 
```bash
yellow
```

### Quoting
- When a command is interpreted by the shell, the shell interprets ALL special characters (command line parsing)
- To ensure that a special character is interpreted by the command, and not by the shell, use quoting
- Quoting is used to treat special characters literally
- If a string of characters is surrounded by single quotation marks `' '`, then ALL characters will be stripped of their special meaning
    - e.g. `echo 'Hello $USER'` -> `Hello $USER`
- If surrounded by double quotes, it will interpolate
    - e.g. `echo 'Hello $USER'` -> `Hello John`
- Best practice: Use single quotes, unless you specifically need parameter/command/arithmetic substitution

### Script Arguments
- `$0` refers to the script name itself
- Arguments stored in any `$1-x` cannot be changed (read-only)
- You would need `${nn}` to access arguments beyond 9
- Use `$@` to refer to all the arguments provided
- Use `$#` to count the amount of arguments provided
- Use `$*` if you need a single string that contains all arguments

### Using `shift` to access args greater than `$9`
- Using `shift` will pop the first value from the list of args, so then `$1` would refer to what was previously the 2nd element in the list, etc.
- Use mainly for compatability with other shells, otherwise, always prefer the `${nn}` method

### String Verification

- Check if a string is empty
```bash
[[ -z $1 ]] && echo "var is empty"
```

- Check for specific patterns
```bash
# If string doesn't start with a letter a-z, then echo to the user to let them know
[[ $1=='[a-z]*' ]] || echo $1 does not start with a letter
```

### Using Here Documents

- Useful for replacing `echo` for long texts that need to be displayed
- Use it in a script if a command is called that opens its own prompt (such as an FTP client interface)
- In a here document, I/O redirection is used to feed a command list into an interactive program (`ftp` or `cat`)
    - Say you wanna use `ftp` to download some files, but in order to do so, you need to engage in an interactive prompt. Using the `>>` operator, you can feed it a command list, and it'll do whatever actions you want it to

```bash
# This script will output/redirect all of the text up until it reaches the string 'End-of-message'
cat <<End-of-message
-----------
Line 1
Line 2
Line 3
-----------
End-of-message
```


```bash
# Get into the lftp CLI, input the list of commands until we reach the 'EndOfSession` string. This will download the file you want, and then you continue normally in your script.
lftp localhost >>EndOfSession
ls
get hosts
bye
EndOfSession

echo The file has been downloaded!
```
