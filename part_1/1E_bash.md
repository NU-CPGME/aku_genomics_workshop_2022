# Basic shell programming

### August 2022

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)*  

---

The shell is the command processor that has been running all of the commands that we've been working with so far. It also functions as a programming language and learning a few tricks can vastly improve your productivity and make some repetitive tasks easier. There are a few different types of shell programs, but the most common is Bash (Bourne Again SHell). The default shell on Mac has been changed to Zsh. There are some prominent differences, but the basic functions that we will use most are the same.

## Section 1 - Loops

One of the functions of Bash that you will use most is loops. These allow you to perform functions mutiple times or on groups of files.

Loops start with a `for` statement and then are followed by a variable name. Usually I use `i` but you can name the variable whatever you want. The next part of the function is `in` followed by the list of variables you want to loop over with a space in between each. 
The next line should start with `do` which signals the start of the functions you want to loop over.
Each subsequent line should contain a function. If you want to use the variable in the function, then add a dollar sign in front of the variable name. Since we used `i`, use `$i` in the function.
Finally, end the loop with `done`.

Example:

```
for i in 1 2 3 A B C  
do  
echo $i  
done
```
The `echo` function prints something to the screen.


Output:

```
1
2
3
A
B
C
```


A more complicated example (leading spaces added for readability only):

```
for file in Ecoli.fasta Kleb.fasta 
do
    echo "Working on $file"
    quast $file > $file.quast.txt
    head -n 5 $file.quast.txt >> all_results.txt
    mv $file finished/
done
```

You can also use the wildcard symbol `*` to auto-generate a variable list instead of typing everything out.

For example, if you have several files in a directory named "genomes" ending with .fasta and you want to perform the same function on all of them, you could start your loop with:

```
for i in genomes/*.fasta
do
```


## Section 2 - Getting loop variables from a file

Sometimes all the variables you want to use will be in a file. You can use the `while` and `read` functions to use these variables in loops.

For example, you have a file named "variables.txt" that looks like this:

```
One
Two
Three
```

To use these variables in your loop, use the `while read` functions:

```
while read var
do
    echo $var
done < variables.txt
```

The `read` function also allows you to use multiple variables in each loop. Just separate them by white space (space or tab) in the variable file.

variables_multi.txt:
```
apples Kevin
pears Suzy
figs John
```

Multiple variable loop example:

```
while read food name
do
    echo "$name likes to eat $food"
done
```

The output will be:

```
Kevin likes to eat apples
Suzy likes to eat pears
John likes to eat figs
```

## Section 3 - Shell script files

If there are sets of commands or loops that you use repeatedly, you probably don't want to type them over and over again. You can solve this by creating shell script files that contain all the commands you would usually type out one at a time. 

For example, you could create a simple text file using a text editor or `nano` called "my_first_script.sh" that contains the following commands:

```
echo "Working hard" > test_file.txt
cp test_file.txt new_file.txt
echo "Hardly working" >> new_file.txt
cat new_file.txt
```

Then to run the commands in that file, just type `sh my_first_script.sh` and see what happens.

You can really make your script files flexible by adding the option to put variables on the command line. For example, we can create a new file called "my_second_script.sh" that has these commands:

```
echo "$1" > test_file.txt
cp test_file.txt new_file.txt
echo "$2" >> new_file.txt
cat new_file.txt
```
 
 The `$1` and `$2` variables refer to the first and second arguments on the command line, respetively. So now if you run the following command: 

```
sh my_second_script.sh open shut
```

You'll see how the arguments on the command line get integrated into the output.

Creating shell scripts like this is one way to start creating, maintaining, and sharing bioinformatic or other analyitcal pipelines. 
 

---

#[Back to table of contents](../README.md)

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.