---
title: 'Batch processing'
description: 'Most shell commands will process many files at once. This chapter will show you how to make your own pipelines do that. Along the way, you will see how the shell uses variables to store information.'
---

## How does the shell store information?

```yaml
type: MultipleChoiceExercise 
lang: shell
xp: 50 
skills: 1
key: e4d5f4adea   
```


Like other programs, the shell stores information in variables.
Some of these,
called **environment variables**,
are available all the time.
Environment variables' names are conventionally written in upper case,
and a few of the more commonly-used ones are shown below.

| Variable | Purpose                           | Value                 |
|----------|-----------------------------------|-----------------------|
| `HOME`   | User's home directory             | `/home/repl`          |
| `PWD `   | Present working directory         | Same as `pwd` command |
| `SHELL`  | Which shell program is being used | `/bin/bash`           |
| `USER`   | User's ID                         | `repl`                |

To get a complete list (which is quite long),
you can type `set` in the shell.

<hr>

Use `set` and `grep` with a pipe to display the value of `HISTFILESIZE`,
which determines how many old commands are stored in your command history.
What is its value?


`@instructions`
- 10
- 500
- [2000]
- The variable is not there.

`@hint`
Use `set | grep HISTFILESIZE` to get the line you need.

`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}

```


`@sct`

```{python}
err1 = "No: the shell records more history than that."
err2 = "No: the shell records more history than that."
correct3 = "Correct: the shell saves 2000 old commands by default on this system."
err4 = "No: the variable `HISTFILESIZE` is there."
Ex().has_chosen(3, [err1, err2, correct3, err4])
```


`@possible_answers`


`@feedback`


---

## How can I print a variable's value?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: afae0f33a7   
```


A simpler way to find a variable's value is to use a command called `echo`,
which prints its arguments:

```{shell}
echo hello DataCamp!
```
```
hello DataCamp!
```

If you try to use it to print a variable's value like this:

```{shell}
echo USER
```
```
USER
```

it will print the variable's name.
To get the variable's value,
you must put a dollar sign `$` in front of it:

```{shell}
echo $USER
```
```
repl
```

This is true everywhere:
to get the value of a variable called `X`,
you must write `$X`.
(This is so that the shell can tell whether you mean "a file named X"
or "the value of a variable named X".)


`@instructions`
The variable `OSTYPE` holds the name of the kind of operating system you are using.
Display its value using `echo`.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
echo $OSTYPE
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        has_expr_output(),
        multi(
            has_code('echo', incorrect_msg="You should use `echo` in your command."),
            has_code('OSTYPE', incorrect_msg="You should use `OSTYPE` in your command."),
            has_code('$', incorrect_msg="Make sure to prepend `OSTYPE` by a `$`")
        )
    )
)
Ex().success_msg("You're off to a good start. Let's carry on!")
```


`@possible_answers`


`@feedback`


---

## How else does the shell store information?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: e925da48e4   
```


The other kind of variable is called a **shell variable**,
which is like a local variable in a programming language.

To create a shell variable,
you simply assign a value to a name:

```{shell}
training=seasonal/summer.csv
```

*without* any spaces before or after the `=` sign.
Once you have done this,
you can check the variable's value with:

```{shell}
echo $training
```
```
seasonal/summer.csv
```


`@instructions`


`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}

```


`@sct`

```{shell}

```


`@possible_answers`


`@feedback`


---

## How can I repeat a command many times?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: 920d1887e3   
```


Shell variables are also used in **loops**,
which repeat commands many times.
If we run this command:

```{shell}
for suffix in gif jpg png; do echo $suffix; done
```

it produces:

```
gif
jpg
png
```

The loop's parts are:

1. The skeleton `for` ...variable... `in` ...list... `; do` ...body... `; done`
2. The list of things the loop is to process (in our case, the words `gif`, `jpg`, and `png`).
3. The variable that keeps track of which thing the loop is currently processing (in our case, `suffix`).
4. The body of the loop that does the processing (in our case, `echo $suffix`).

Notice that the body uses `$suffix` to get the variable's value instead of just `suffix`,
just like it does with any other shell variable.
Also notice where the semi-colons go:
the first one comes between the list and the keyword `do`,
and the second comes between the body and the keyword `done`.


`@instructions`
Modify the loop so that it prints:

```
docx
odt
pdf
```

Please use `suffix` as the name of the loop variable.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
for suffix in docx odt pdf; do echo $suffix; done
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        has_expr_output(),
        has_code(r'\s*for\s+suffix\s+in\s+docx\s+odt\s+pdf\s*;\s*do\s+echo\s+\$suffix\s*;\s*done\s*',
                 incorrect_msg='Change the list of suffix names that the loop operatores on: `docx odt pdf` instead of `gif jpg png`.')
    )
)
```


`@possible_answers`


`@feedback`


---

## How can I repeat a command once for each file?

```yaml
type: ConsoleExercise 
xp: 0 
key: 8468b70a71   
```


You can always type in the names of the files you want to process when writing the loop,
but it's usually better to use wildcards.
Try running this loop in the console:

```{shell}
for filename in seasonal/*.csv; do echo $filename; done
```

It prints:

```
seasonal/autumn.csv
seasonal/spring.csv
seasonal/summer.csv
seasonal/winter.csv
```

because the shell expands `seasonal/*.csv` to be a list of four filenames
before it runs the loop.


`@instructions`
Modify the wildcard expression to `people/*`
so that the loop prints the names of the files in the `people` directory
regardless of what suffix they do or don't have.
Please use `filename` as the name of your loop variable.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{bash}
for filename in people/*; do echo $filename; done
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        has_expr_output(),
        has_code(r'\s*for\s+filename\s+in\s+([~.]/)?people/\*\s*;\s*do\s+echo\s+\$filename\s*;\s*done\s*',
                 incorrect_msg='The example uses `seasonal/*.csv`; you should use `people/*` to get the name of all the files in the `people` directory.')
    )
)
```


`@possible_answers`


`@feedback`


---

## How can I record the names of a set of files?

```yaml
type: MultipleChoiceExercise 
lang: shell
xp: 50 
skills: 1
key: 153ca10317   
```


People often set a variable using a wildcard expression to record a list of filenames.
For example,
if you define `datasets` like this:

```{shell}
datasets=seasonal/*.csv
```

you can display the files' names later using:

```{shell}
for filename in $datasets; do echo $filename; done
```

This saves typing and makes errors less likely.

<hr>

If you run these two commands in your home directory,
how many lines of output will they print?

```{shell}
files=seasonal/*.csv
for f in $files; do echo $f; done
```


`@instructions`
- None: since `files` is defined on a separate line, it has no value in the second line.
- One: the word "files".
- Four: the names of all four seasonal data files.

`@hint`
Remember that `X` on its own is just "X", while `$X` is the value of the variable `X`.

`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}

```


`@sct`

```{python}
err1 = "No: you do not have to define a variable on the same line you use it."
err2 = "No: this example defines and uses the variable `files` in the same shell."
correct3 = "Correct."
Ex().has_chosen(3, [err1, err2, correct3])
```


`@possible_answers`


`@feedback`


---

## A variable's name versus its value

```yaml
type: PureMultipleChoiceExercise 
lang: bash
xp: 50 
key: 4fcfb63c4f   
```


A common mistake is to forget to use `$` before the name of a variable.
When you do this,
the shell uses the name you have typed
rather than the value of that variable.

A more common mistake for experienced users is to mis-type the variable's name.
For example,
if you define `datasets` like this:

```{shell}
datasets=seasonal/*.csv
```

and then type:

```{shell}
echo $datsets
```

the shell doesn't print anything,
because `datsets` (without the second "a") isn't defined.

<hr>

If you were to run these two commands in your home directory,
what output would be printed?

```{shell}
files=seasonal/*.csv
for f in files; do echo $f; done
```

(Read the first part of the loop carefully before answering.)


`@instructions`


`@hint`
Remember that `X` on its own is just "X", while `$X` is the value of the variable `X`.

`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}

```


`@sct`

```{shell}

```


`@possible_answers`
- [One line: the word "files".]
- Four lines: the names of all four seasonal data files.
- Four blank lines: the variable `f` isn't assigned a value.

`@feedback`
- Correct: the loop uses `files` instead of `$files`, so the list consists of the word "files".
- No: the loop uses `files` instead of `$files`, so the list consists of the word "files" rather than the expansion of `files`.
- No: the variable `f` is defined automatically by the `for` loop.

---

## How can I run many commands in a single loop?

```yaml
type: ConsoleExercise 
xp: 0 
key: 39b5dcf81a   
```


Printing filenames is useful for debugging,
but the real purpose of loops is to do things with multiple files.
This loop prints the second line of each data file:

```{shell}
for file in seasonal/*.csv; do head -n 2 $file | tail -n 1; done
```

It has the same structure as the other loops you have already seen:
all that's different is that its body is a pipeline of two commands instead of a single command.


`@instructions`
Write a loop that produces the same output as

```{shell}
grep -h 2017-07 seasonal/*.csv
```

but uses a loop to process each file separately.
Please use `file` as the name of the loop variable,
and remember that the `-h` flag used above tells `grep` *not* to print filenames in the output.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{bash}
for file in seasonal/*.csv; do grep 2017-07 $file; done
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        has_expr_output(),
        has_code(r'\s*for\s+file\s+in\s+seasonal/\*\.csv\s*;\s*do\s+grep(\s+-h)?\s+2017-07\s+\$file\s*;\s*done\s*', incorrect_msg='Use `grep 2017-07 $file` as the body of the loop.')
    )
)
```


`@possible_answers`


`@feedback`


---

## Why shouldn't I use spaces in filenames?

```yaml
type: PureMultipleChoiceExercise 
lang: bash
xp: 50 
key: b974b7f45a   
```


It's easy and sensible to give files multi-word names like `July 2017.csv`
when you are using a graphical file explorer.
However,
this causes problems when you are working in the shell.
For example,
suppose you wanted to rename `July 2017.csv` to be `2017 July data.csv`.
You cannot type:

```{shell}
mv July 2017.csv 2017 July data.csv
```

because it looks to the shell as though you are trying to move
four files called `July`, `2017.csv`, `2017`, and `July` (again)
into a directory called `data.csv`.
Instead,
you have to quote the files' names
so that the shell treats each one as a single parameter:

```{shell}
mv 'July 2017.csv' '2017 July data.csv'
```

<hr>

If you have two files called `current.csv` and `last year.csv`
(with a space in its name)
and you type:

```{shell}
rm current.csv last year.csv
```

what will happen:


`@instructions`


`@hint`
What would you think was going to happen if someone showed you the command and you didn't know what files existed?

`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}

```


`@sct`

```{shell}

```


`@possible_answers`
- The shell will print an error message because `last` and `year.csv` do not exist.
- The shell will delete `current.csv`.
- [Both of the above.]
- Nothing.

`@feedback`
- Yes, but that's not all.
- Yes, but that's not all.
- Correct.
- Unfortunately not.

---

## How can I do many things in a single loop?

```yaml
type: MultipleChoiceExercise 
lang: shell
xp: 50 
skills: 1
key: f6d0530991   
```


The loops you have seen so far all have a single command or pipeline in their body,
but a loop can contain any number of commands.
To tell the shell where one ends and the next begins,
you must separate them with semi-colons:

```{shell}
for f in seasonal/*.csv; do echo $f; head -n 2 $f | tail -n 1; done
```

```
seasonal/autumn.csv
2017-01-05,canine
seasonal/spring.csv
2017-01-25,wisdom
seasonal/summer.csv
2017-01-11,canine
seasonal/winter.csv
2017-01-03,bicuspid
```

<hr>

Suppose you forget the semi-colon between the `echo` and `head` commands in the previous loop,
so that you ask the shell to run:

```{shell}
for f in seasonal/*.csv; do echo $f head -n 2 $f | tail -n 1; done
```

What will the shell do?


`@instructions`
- Print an error message.
- Print one line for each of the four files.
- Print one line for `autumn.csv` (the first file).
- Print the last line of each file.

`@hint`
You can pipe the output of `echo` to `tail`.

`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}

```


`@sct`

```{python}
err1 = "No: the loop will run, it just won't do something sensible."
correct2 = "Yes: `echo` produces one line that includes the filename twice, which `tail` then copies."
err3 = "No: the loop runs one for each of the four filenames."
err4 = "No: the input of `tail` is the output of `echo` for each filename."
Ex().has_chosen(2, [err1, correct2, err3, err4])
```


`@possible_answers`


`@feedback`

