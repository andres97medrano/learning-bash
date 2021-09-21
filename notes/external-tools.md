### grep
- flexible tool used to search for text patterns based on regex
- basic usage: `grep <string/regex> <filename/file-pattern>`
    - [case insensitive] `grep -i`
        -  `grep -i 'what' *`
    - [exclude lines that match pattern] `grep -v`
        -  `grep -v 'what' *`
    - [search in subdirectories as well - recursive] `grep -r`
        -  `grep -r 'what' *`
    - [use multiple regex - egrep] `grep -e` 
        -  `grep -e 'what' -e 'something' -e 'else' *`
    - [show X amount of lines _after_ matching regex] `grep -AX`
    - [show X amount of lines _before_ matching regex] `grep -BX`

e.g. 
```bash
ls
cool-dir/ another-dir/ hello_world.txt

cat hello_world.txt
Hello, world

# Look for string 'Hello' in `hello_world.txt`
grep 'Hello' hello_world.txt
Hello, world

# Look for string 'hello' in `hello_world.txt`
grep 'Hello' hello_world.txt
```

### test
- allows you to test many items
    - [expression] `test (ls /etc/hosts)`
    - [string] `test -z $1`
    - [integers] `test $1 = 6`
    - [file comparisons] `test file -nt file2`
    - [file properties] `test -x file1`
- evaluates the expression and returns 0 if evals to True, or 1 if evals to False
    - if no expression, also returns 1

- `test -z $1` and `[ -z $1 ]` are equivalent
    - ``[[ -z $1 ]]` new and improved version of `[...]`
        - it has `&&` and `||` built-in, but not as universal yet
        - prefer to use double-bracket unless you care about keeping compatability with older shells

### cut and sort
- `cut` is used to filter a specific column or field out of a line
    - `-f` is to specify what column you want, you can use multiple times to get multiple columns
    - `-d` is to specify the delimiter
- `sort` is used to sort data in a specific order
    - 
- both are usually used together

Say, I have this file named `dogs.txt` with dog names, their age, and breed.
```
rocky,1,maltipoo
princess,3,chihuahua
ace,2,cavapoo
rex,5,rottweiler
simba,12,chihuahua mix
honey,6,pitbull
motita,12,poodle mix
luna,2,maltipoo
```

We can do the following operations:
```bash
# Show column 1 (names)
> cut -f 1 -d , resources/dogs.txt
rocky
princess
ace
rex
simba
honey
motita
luna

# Get column 2 (age), and sort by number (-n)
> cut -f 2 -d , resources/dogs.txt | sort -n
1
2
2
3
5
6
12
12

# Sort the list by column 2 as number (-n) (age)
>  sort -n -k2 -t , resources/dogs.txt
rocky,1,maltipoo
ace,2,cavapoo
luna,2,maltipoo
princess,3,chihuahua
rex,5,rottweiler
honey,6,pitbull
motita,12,poodle mix
simba,12,chihuahua mix

# Sort the list in reverse order as number (-rn) by column 2 (age)
>  sort -rn -k2 -t , resources/dogs.txt
simba,12,chihuahua mix
motita,12,poodle mix
honey,6,pitbull
rex,5,rottweiler
princess,3,chihuahua
luna,2,maltipoo
ace,2,cavapoo
rocky,1,maltipoo
```

- The advantage of using `sort` over an advanced utility such as `awk` is that it's relatively easy to use

### `tail` and `head`
- `tail` is used to display the last X amount of line(s) in a file
- `head` is used to display the first X amount of line(s) in a file
- if you don't specify X amount, it specifies 10 by default

```bash
# Shows the first two lines in the file
> head -2 resources/dogs.txt
rocky,1,maltipoo
princess,3,chihuahua

# Shows the last 3 lines in the file
> tail -3 resources/dogs.txt
honey,6,pitbull
motita,12,poodle mix
luna,2,maltipoo
# You can also combine the commands..

# Shows the fifth line in the file
> head -5 resources/dogs.txt | tail -1
simba,12,chihuahua mix
```

### `sed`
- Streamline editor
- It's a programming language with many features
- There's books on `sed` that are over 400 pages, so it's a pretty comprehensive tool
    - use for a limited set of operations, unless you know what you're doing

`sed -n 5p myfile.txt`
- print line 5 to stdout

`sed -i s/old-text/new-text/g myfile.txt`
- replace 'old-text' with 'new-text' in file within global scope
- note: didn't get this working; it said "sed: -I or -i may not be used with stdin"

### `awk`
- this is awk
- `awk` is a very rich language
- use case is to filter information from text files based on regex patterns
- basic usage: `awk '/search pattern/ {Actions}' <file>`

```bash
> cat resources/users.txt
pablo:1
ricky:2
nicole:3
rambo:4
silverio:5

# print the user ID belonging to 'silverio' 
>  awk -F : '/silverio/ { print $2 }' resources/users.txt 
5

# print the dogs who are greater than 3yo (column 2)
> awk -F , '$2 > 3' resources/dogs.txt
rex,5,rottweiler
simba,12,chihuahua mix
honey,6,pitbull
motita,12,poodle mix

# print the dogs who are of type 'mix' (column 3)
> awk -F , '$3 ~/mix/' resources/dogs.txt 
simba,12,chihuahua mix
motita,12,poodle mix
```

### `tr`
- `tr` helps in transforming strings
```bash
> echo uppercasemedawg | tr '[:lower:]' '[:upper:]'
UPPERCASEMEDAWG
```