---
title: 'Creating new tools'
description: 'History lets you repeat things with just a few keystrokes, and pipes let you combine existing commands to create new ones. In this chapter, you will see how to go one step further and create new commands of your own.'
---

## How can I edit a file?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: 39eee3cfc0   
```


Unix has a bewildering variety of text editors.
For this course,
we will use a simple one called Nano.
If you type `nano filename`,
it will open `filename` for editing
(or create it if it doesn't already exist).
You can move around with the arrow keys,
delete characters using backspace,
and do other operations with control-key combinations:

- Ctrl-K: delete a line.
- Ctrl-U: un-delete a line.
- Ctrl-O: save the file ('O' stands for 'output').
- Ctrl-X: exit the editor.


`@instructions`
Run `nano names.txt` to edit a new file in your home directory
and enter the following four lines:

```
Lovelace
Hopper
Johnson
Wilson
```

To save what you have written,
type Ctrl-O to write the file out,
then Enter to confirm the filename,
then Ctrl-X and Enter to exit the editor.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
# This solution uses `cp` instead of `nano`
# because our automated tests can't edit files interactively.
cp /solutions/names.txt /home/repl
```


`@sct`

```{python}
patt = "Have you included the line `%s` in the `names.txt` file? Use `nano names.txt` again to update your file. Use Ctrl-O to save and Ctrl-X to exit."
Ex().multi(
    has_cwd('/home/repl'),
    check_file('/home/repl/names.txt').multi(
        has_code('Lovelace', incorrect_msg=patt%'Lovelace'),
        has_code('Hopper', incorrect_msg=patt%'Hopper'),
        has_code('Johnson', incorrect_msg=patt%'Johnson'),
        has_code('Wilson', incorrect_msg=patt%'Wilson')
    )
)
Ex().success_msg("Well done! Off to the next one!")
```


`@possible_answers`


`@feedback`


---

## How can I record what I just did?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: 80c3532985   
```


When you are doing a complex analysis,
you will often want to keep a record of the commands you used.
You can do this with the tools you have already seen:

1. Run `history`.
2. Pipe its output to `tail -n 10` (or however many recent steps you want to save).
3. Redirect that to a file called something like `figure-5.history`.

This is better than writing things down in a lab notebook
because it is guaranteed not to miss any steps.
It also illustrates the central idea of the shell:
simple tools that produce and consume lines of text
can be combined in a wide variety of ways
to solve a broad range of problems.


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

## How can I save commands to re-run later?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: 4507a0dbd8   
```


You have been using the shell interactively so far.
But since the commands you type in are just text,
you can store them in files for the shell to run over and over again.
To start exploring this powerful capability,
put the following command in a file called `headers.sh`:

```{shell}
head -n 1 seasonal/*.csv
```

This command selects the first row from each of the CSV files in the `seasonal` directory.
Once you have created this file,
you can run it by typing:

```{shell}
bash headers.sh
```

This tells the shell (which is just a program called `bash`)
to run the commands contained in the file `headers.sh`,
which produces the same output as running the commands directly.


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

## How can I re-use pipes?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: da13667750   
```


A file full of shell commands is called a ***shell script**,
or sometimes just a "script" for short. Scripts don't have to have names ending in `.sh`,
but this lesson will use that convention
to help you keep track of which files are scripts.

Scripts can also contain pipes.
For example,
if `all-dates.sh` contains this line:

```{shell}
cut -d , -f 1 seasonal/*.csv | grep -v Date | sort | uniq
```

then:

```{shell}
bash all-dates.sh > dates.out
```

will extract the unique dates from the seasonal data files
and save them in `dates.out`.


`@instructions`


`@hint`


`@pre_exercise_code`

```{python}
import shutil
shutil.copyfile('/solutions/teeth-start.sh', 'teeth.sh')
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

## How can I pass filenames to scripts?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: c2623b9c14   
```


A script that processes specific files is useful as a record of what you did,
but one that allows you to process any files you want is more useful.
To support this,
you can use the special expression `$@` (dollar sign immediately followed by at-sign)
to mean "all of the command-line parameters given to the script".
For example,
if `unique-lines.sh` contains this:

```{shell}
sort $@ | uniq
```

then when you run:

```{shell}
bash unique-lines.sh seasonal/summer.csv
```

the shell replaces `$@` with `seasonal/summer.csv` and processes one file.
If you run this:

```{shell}
bash unique-lines.sh seasonal/summer.csv seasonal/autumn.csv
```

it processes two data files,
and so on.


`@instructions`


`@hint`


`@pre_exercise_code`

```{python}
import shutil
shutil.copyfile('/solutions/count-records-start.sh', 'count-records.sh')
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

## How can I process a single argument?

```yaml
type: PureMultipleChoiceExercise 
lang: bash
xp: 50 
key: 4092cb4cda   
```


As well as `$@`,
the shell lets you use `$1`, `$2`, and so on to refer to specific command-line parameters.
You can use this to write commands that feel simpler or more natural than the shell's.
For example,
you can create a script called `column.sh` that selects a single column from a CSV file
when the user provides the filename as the first parameter and the column as the second:

```{shell}
cut -d , -f $2 $1
```

and then run it using:

```{shell}
bash column.sh seasonal/autumn.csv 1
```

Notice how the script uses the two parameters in reverse order.

<hr>

The script `get-field.sh` is supposed to take a filename,
the number of the row to select,
the number of the column to select,
and print just that field from a CSV file.
For example:

```
bash get-field.sh seasonal/summer.csv 4 2
```

should select the second field from line 4 of `seasonal/summer.csv`.
Which of the following commands should be put in `get-field.sh` to do that?


`@instructions`


`@hint`
Remember that command-line parameters are numbered left to right.

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
- `head -n $1 $2 | tail -n 1 | cut -d , -f $3`
- [`head -n $2 $1 | tail -n 1 | cut -d , -f $3`]
- `head -n $3 $1 | tail -n 1 | cut -d , -f $2`
- `head -n $2 $3 | tail -n 1 | cut -d , -f $1`

`@feedback`
- No: that will try to use the filename as the number of lines to select with `head`.
- Correct!
- No: that will try to use the column number as the line number and vice versa.
- No: that will use the field number as the filename and vice versa.

---

## How can one shell script do many things?

```yaml
type: TabConsoleExercise 
xp: 0 
key: 846bc70e9d   
```


Our shells scripts so far have had a single command or pipe,
but a script can contain many lines of commands.
For example,
you can create one that tells you how many records are in the shortest and longest of your data files,
i.e.,
the range of your datasets' lengths.


`@instructions`


`@hint`


`@pre_exercise_code`

```{python}
import shutil
shutil.copyfile('/solutions/range-start-1.sh', 'range.sh')
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

## How can I write loops in a shell script?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: 6be8ca6009   
```


Shell scripts can also contain loops.
You can write them using semi-colons,
or split them across lines without semi-colons to make them more readable:

```{shell}
# Print the first and last data records of each file.
for filename in $@
do
    head -n 2 $filename | tail -n 1
    tail -n 1 $filename
done
```

(You don't have to indent the commands inside the loop,
but doing so makes things clearer.)

The first line of this script is a **comment**
to tell readers what the script does.
Comments start with the `#` character and run to the end of the line.
Your future self will thank you for adding brief explanations like the one shown here
to every script you write.


`@instructions`


`@hint`


`@pre_exercise_code`

```{python}
import shutil
shutil.copyfile('/solutions/date-range-start.sh', 'date-range.sh')
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

## What happens when I don't provide filenames?

```yaml
type: MultipleChoiceExercise 
lang: shell
xp: 50 
skills: 1
key: 8a162c4d54   
```


A common mistake in shell scripts (and interactive commands) is to put filenames in the wrong place.
If you type:

```{shell}
tail -n 3
```

then since `tail` hasn't been given any filenames,
it waits to read input from your keyboard.
This means that if you type:

```{shell}
head -n 5 | tail -n 3 somefile.txt
```

then `tail` goes ahead and prints the last three lines of `somefile.txt`,
but `head` waits forever for keyboard input,
since it wasn't given a filename and there isn't anything ahead of it in the pipeline.

<hr>

Suppose you do accidentally type:

```{shell}
head -n 5 | tail -n 3 somefile.txt
```

What should you do next?


`@instructions`
- Wait 10 seconds for `head` to time out.
- Type `somefile.txt` and press Enter to give `head` some input.
- Use Ctrl-C to stop the running `head` program.

`@hint`
What does `head` do if it doesn't have a filename and nothing is upstream from it?

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
a1 = 'No, commands will not time out.'
a2 = 'No, that will give `head` the text `somefile.txt` to process, but then it will hang up waiting for still more input.'
a3 = 'Yes! You should use Ctrl-C to stop a running program.'
Ex().has_chosen(3, [a1, a2, a3])
```


`@possible_answers`


`@feedback`

