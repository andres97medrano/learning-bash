### Substitution Operators
- aka "String Operators"
- allow you to manipulate values in an easy way

 ```bash

# if VAR exists, use its value, if not, _return_ the value "word"
${VAR:-word}

# if VAR exists, use its value, if not, _set_ its value to "word"
${VAR:=word}

# if VAR exists, show its value, if not, _display_ VAR followed by message
${VAR:?message}

# if VAR exists, show the substring of it starting at the offset, for `length` amount of characters
${VAR:offset:length}
 ```

#### Examples
```bash
DATE=
echo $DATE
# >
echo ${DATE:-hi there}
# > hi there
echo $DATE
# >
echo ${DATE:=cool}
# > cool
echo $DATE
# > cool
DATE=
echo ${DATE:?variable not set}
# > bash: DATE: variable not set
DATE=$( date +%m-%d-%y ) # 09-16-21
echo the day is ${DATE:3:2}
# > the day is 16
```

### Pattern Matching
- use pattern matching to _remove_ patterns from a variable

#### Examples
Single-hash: find _first_ occurrence of pattern, and return everything after that point
```bash
# This removes everything up to the first slash in string
FNAME=/usr/bin/blah
echo ${FNAME#*/}
# > usr/bin/blah
```
Double-hash: find _last_ occurrence of pattern, and return everything after that point
```bash
# This removes everything up to the last slash in string
FNAME=/usr/bin/blah
echo ${FNAME##*/}
# > blah
```
For percentage symbols, it's the same concept but starting from right to left

Single-percentage: find the _first_ occurrence of pattern starting at the end of string, and return everything before it
```bash
# This removes everything up to the last slash in string
FNAME=/usr/bin/blah
echo ${FNAME%/*}
# > /usr/bin
```
Double-percentage: find the _last_ occurrence of pattern starting at the end of string, and return everything before it
```bash
# This removes everything up to the last slash in string
FNAME=/usr/bin/blah
echo ${FNAME%%/*}
# > 
```

### Regular Expressions
- search patterns that can be used by some utilities such as **grep**, **awk**, and **sed**
    - fun fact: **grep** stands for General Regular Expression Parser
- regex expressions are not the same as shell wildcards
- in practice: when using regex, put them between strong quotes so that the shell won't interpret them 

#### Common Usages
```bash
# line starts with 'text'
> ^text

# line ends with 'text'
> text$

# wildcard (match any character)
> .

# match a character set
> [abc], [a-c]

# match 0+ number of previous character
> *

# match exactly 2 of previous character
> \{2\}

# match a min of 1 and max of 3 of previous character
> \{1,3\}

# match 0 or 1 of the previous character (optional)
> colou?r
```

### Bash Calculating
- internal calculation:
```bash
echo $(( 1 + 1 ))
```
- external calculation using `let` (execute from script):
```bash
# Here, $1 and $3 are your numbers, and $2 is your operator
# Note that bash does integer operations only
let x = "$1 $2 $3"
echo $x
```
- external calculation using `bc`
    - `bc` is an external shell utility calculator using its own interactive shell
    - the main benefit is that it can do operations with more than just integers
    - uses:
    ```bash
    # pipe to bc
    echo "scale=2; 10/3" | bc
    > 3.33

    # save to VAR
    NUMBER=$(echo "scale=2; 10/3" | bc)
    echo $NUMBER
    > 3.33
    ```