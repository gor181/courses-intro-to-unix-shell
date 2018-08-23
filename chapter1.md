---
title: 'Manipulating files and directories'
description: 'This chapter is a brief introduction to the Unix shell.  You''ll learn why it is still in use after almost fifty years, how it compares to the graphical tools you may be more familiar with, how to move around in the shell, and how to create, modify, and delete files and folders.'
---

## How does the shell compare to a desktop interface?

```yaml
type: PureMultipleChoiceExercise 
lang: bash
xp: 50 
key: badd717ea4   
```


An operating system like Windows, Linux, or Mac OS is a special kind of program.
It controls the computer's processor, hard drive, and network connection,
but its most important job is to run other programs.

Since human beings aren't digital,
they need an interface to interact with the operating system.
The most common one these days is a graphical file explorer,
which translates clicks and double-clicks into commands to open files and run programs.
Before computers had graphical displays,
though,
people typed instructions into a program called a **command-line shell**.
Each time a command is entered,
the shell runs some other programs,
prints their output in human-readable form,
and then displays a *prompt* to signal that it's ready to accept the next command.
(Its name comes from the notion that it's the "outer shell" of the computer.)

Typing commands instead of clicking and dragging may seem clumsy at first,
but as you will see,
once you start spelling out what you want the computer to do,
you can combine old commands to create new ones
and automate repetitive operations
with just a few keystrokes.

<hr>
What is the relationship between the graphical file explorer that most people use and the command-line shell?


`@instructions`


`@hint`
Remember that a user can only interact with an operating system through a program.

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
- The file explorer lets you view and edit files, while the shell lets you run programs.
- The file explorer is built on top of the shell.
- The shell is part of the operating system, while the file explorer is separate.
- [They are both interfaces for issuing commands to the operating system.]

`@feedback`
- Both allow you to view and edit files and run programs.
- Graphical file explorers and the shell both call the same underlying operating system functions.
- The shell and the file explorer are both programs that translate user commands (typed or clicked) into calls to the operating system.
- Correct! Both take the user's commands (whether typed or clicked) and send them to the operating system.

---

## Where am I?

```yaml
type: MultipleChoiceExercise 
lang: shell
xp: 50 
skills: 1
key: 7c1481dbd3   
```


The **filesystem** manages files and directories (or folders).
Each is identified by an **absolute path**
that shows how to reach it from the filesystem's **root directory**:
`/home/repl` is the directory `repl` in the directory `home`,
while `/home/repl/course.txt` is a file `course.txt` in that directory,
and `/` on its own is the root directory.

To find out where you are in the filesystem,
run the command `pwd`
(short for "**p**rint **w**orking **d**irectory").
This prints the absolute path of your **current working directory**,
which is where the shell runs commands and looks for files by default.

<hr>
Run `pwd`.
Where are you right now?


`@instructions`
- `/home`
- `/repl`
- `/home/repl`

`@hint`
Unix systems typically place all users' home directories underneath `/home`.

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
err = "That is not the correct path."
correct = "Correct - you are in `/home/repl`."

Ex().has_chosen(3, [err, err, correct])
```


`@possible_answers`


`@feedback`


---

## How can I identify files and directories?

```yaml
type: MultipleChoiceExercise 
lang: shell
xp: 50 
skills: 1
key: f5b0499835   
```


`pwd` tells you where you are.
To find out what's there,
type `ls` (which is short for "listing") and press the enter key.
On its own,
`ls` lists the contents of your current directory
(the one displayed by `pwd`).
If you add the names of some files,
`ls` will list them,
and if you add the names of directories,
it will list their contents.
For example,
`ls /home/repl` shows you what's in your starting directory
(usually called your **home directory**).

<hr>
Use `ls` with an appropriate argument to list the files in the directory `/home/repl/seasonal`
(which holds information on dental surgeries by date, broken down by season).
Which of these files is *not* in that directory?


`@instructions`
- `autumn.csv`
- `fall.csv`
- `spring.csv`
- `winter.csv`

`@hint`
If you give `ls` a path, it shows what's in that path.

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
err = "That file is in the `seasonal` directory."
correct = "Correct - that file is *not* in the `seasonal` directory."

Ex().has_chosen(2, [err, correct, err, err])
```


`@possible_answers`


`@feedback`


---

## How else can I identify files and directories?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: a766184b59   
```


An absolute path is like a latitude and longitude:
it specifies the same thing no matter where you are.
A **relative path**,
on the other hand,
specifies a location starting from where you are:
it's like saying "20 kilometers north".

For example,
if you are in the directory `/home/repl`,
the relative path `seasonal` specifies the same directory as `/home/repl/seasonal`,
while `seasonal/winter.csv` specifies the same file as `/home/repl/seasonal/winter.csv`.
The shell decides if a path is absolute or relative by looking at its first character:
if it begins with `/`, it is absolute,
and if it doesn't,
it is relative.


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

## How can I move to another directory?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: dbdaec5610   
```


Just as you can move around in a file browser by double-clicking on folders,
you can move around in the filesystem using the command `cd`
(which stands for "change directory").

If you type `cd seasonal` and then type `pwd`,
the shell will tell you that you are now in `/home/repl/seasonal`.
If you then run `ls` on its own,
it shows you the contents of `/home/repl/seasonal`,
because that's where you are.
If you want to get back to your home directory `/home/repl`,
you can use the command `cd /home/repl`.


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

## How can I move up a directory?

```yaml
type: PureMultipleChoiceExercise 
lang: shell
xp: 50 
skills: 1
key: 09c717ef76   
```


The **parent** of a directory is the directory above it.
For example, `/home` is the parent of `/home/repl`,
and `/home/repl` is the parent of `/home/repl/seasonal`.
You can always give the absolute path of your parent directory to commands like `cd` and `ls`.
More often,
though,
you will take advantage of the fact that the special path `..`
(two dots with no spaces) means "the directory above the one I'm currently in".
If you are in `/home/repl/seasonal`,
then `cd ..` moves you up to `/home/repl`.
If you use `cd ..` once again,
it puts you in `/home`.
One more `cd ..` puts you in the *root directory* `/`,
which is the very top of the filesystem.
(Remember to put a space between `cd` and `..` - it is a command and a path, not a single four-letter command.)

A single dot on its own, `.`, always means "the current directory",
so `ls` on its own and `ls .` do the same thing,
while `cd .` has no effect
(because it moves you into the directory you're currently in).

One final special path is `~` (the tilde character),
which means "your home directory",
such as `/home/repl`.
No matter where you are,
`ls ~` will always list the contents of your home directory,
and `cd ~` will always take you home.

<hr>
If you are in `/home/repl/seasonal`,
where does `cd ~/../.` take you?


`@instructions`


`@hint`
Trace the path one directory at a time.

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
- `/home/repl`
- [`/home`]
- `/home/repl/seasonal`
- `/` (the root directory)

`@feedback`
- No, but either `~` or `..` on its own would take you there.
- Correct! The path means 'home directory', 'up a level', 'here'.
- No, but `.` on its own would do that.
- No, the final part of the path is `.` (meaning "here") rather than `..` (meaning "up").

---

## How can I copy files?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: 832de9e74c   
```


You will often want to copy files,
move them into other directories to organize them,
or rename them.
One command to do this is `cp`, which is short for "copy".
If `original.txt` is an existing file,
then:

```{shell}
cp original.txt duplicate.txt
```

creates a copy of `original.txt` called `duplicate.txt`.
If there already was a file called `duplicate.txt`,
it is overwritten.
If the last parameter to `cp` is an existing directory,
then a command like:

```{shell}
cp seasonal/autumn.csv seasonal/winter.csv backup
```

copies *all* of the files into that directory.


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

## How can I move a file?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: 663a083a3c   
```


While `cp` copies a file,
`mv` moves it from one directory to another,
just as if you had dragged it in a graphical file browser.
It handles its parameters the same way as `cp`,
so the command:

```{shell}
mv autumn.csv winter.csv ..
```

moves the files `autumn.csv` and `winter.csv` from the current working directory
up one level to its parent directory
(because `..` always refers to the directory above your current location).


`@instructions`
You are in `/home/repl`, which has sub-directories `seasonal` and `backup`.
Using a single command, move `spring.csv` and `summer.csv` from `seasonal` to `backup`.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
mv seasonal/spring.csv seasonal/summer.csv backup
```


`@sct`

```{python}
backup_patt="The file `%s` is not in the `backup` directory. Have you used `mv` correctly? Use two filenames and a directory as parameters to `mv`."
seasonal_patt="The file `%s` is still in the `seasonal` directory. Make sure to move the files with `mv` rather than copying them with `cp`!"
Ex().multi(
    check_file('/home/repl/backup/spring.csv', missing_msg=backup_patt%'spring.csv'),
    check_file('/home/repl/backup/summer.csv', missing_msg=backup_patt%'summer.csv'),
    check_not(check_file('/home/repl/seasonal/spring.csv'), incorrect_msg=seasonal_patt%'spring.csv'),
    check_not(check_file('/home/repl/seasonal/summer.csv'), incorrect_msg=seasonal_patt%'summer.csv')
)
Ex().success_msg("Well done, let's keep this shell train going!")
```


`@possible_answers`


`@feedback`


---

## How can I rename files?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: 001801a652   
```


`mv` can also be used to rename files. If you run:

```{shell}
mv course.txt old-course.txt
```

then the file `course.txt` in the current working directory is "moved" to the file `old-course.txt`.
This is different from the way file browsers work,
but is often handy.

One warning:
just like `cp`,
`mv` will overwrite existing files.
If,
for example,
you already have a file called `old-course.txt`,
then the command shown above will replace it with whatever is in `course.txt`.


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

## How can I delete files?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: 2734680614   
```


We can copy files and move them around;
to delete them,
we use `rm`,
which stands for "remove".
As with `cp` and `mv`,
you can give `rm` the names of as many files as you'd like, so:

```{shell}
rm thesis.txt backup/thesis-2017-08.txt
```

removes both `thesis.txt` and `backup/thesis-2017-08.txt`

`rm` does exactly what its name says,
and it does it right away:
unlike graphical file browsers,
the shell doesn't have a trash can,
so when you type the command above,
your thesis is gone for good.


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

## How can I create and delete directories?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: 63e8fbd0c2   
```


`mv` treats directories the same way it treats files:
if you are in your home directory and run `mv seasonal by-season`,
for example,
`mv` changes the name of the `seasonal` directory to `by-season`.
However,
`rm` works differently.

If you try to `rm` a directory,
the shell prints an error message telling you it can't do that,
primarily to stop you from accidentally deleting an entire directory full of work.
Instead,
you can use a separate command called `rmdir`.
For added safety,
it only works when the directory is empty,
so you must delete the files in a directory *before* you delete the directory.
(Experienced users can use the `-r` option to `rm` to get the same effect;
we will discuss command options in the next chapter.)


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

## Wrapping up

```yaml
type: BulletConsoleExercise 
xp: 0 
key: b1990e9a42   
```


You will often create intermediate files when analyzing data.
Rather than storing them in your home directory,
you can put them in `/tmp`,
which is where people and programs often keep files they only need briefly.
(Note that `/tmp` is immediately below the root directory `/`,
*not* below your home directory.)
This wrap-up exercise will show you how to do that.


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

