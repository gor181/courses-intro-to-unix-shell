---
title: 'Combining tools'
description: 'The real power of the Unix shell lies not in the individual commands, but in how easily they can be combined to do new things. This chapter will show you how to use this power to select the data you want, and introduce commands for sorting values and removing duplicates.'
---

## How can I store a command's output in a file?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: 07a427d50c   
```


All of the tools you have seen so far let you name input files.
Most don't have an option for naming an output file because they don't need one.
Instead,
you can use **redirection** to save any command's output anywhere you want.
If you run this command:

```{shell}
head -n 5 seasonal/summer.csv
```

it prints the first 5 lines of the summer data on the screen.
If you run this command instead:

```{shell}
head -n 5 seasonal/summer.csv > top.csv
```

nothing appears on the screen.
Instead,
`head`'s output is put in a new file called `top.csv`.
You can take a look at that file's contents using `cat`:

```{shell}
cat top.csv
```

The greater-than sign `>` tells the shell to redirect `head`'s output to a file.
It isn't part of the `head` command;
instead,
it works with every shell command that produces output.


`@instructions`
Save the last 5 lines of `seasonal/winter.csv` in a file called `last.csv`.
(Use `tail -n 5` to get the last 5 lines.)

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
tail -n 5 seasonal/winter.csv > last.csv
```


`@sct`

```{python}
from shellwhat_ext import test_cmdline, rxc
patt = "The line `%s` should be in the file `last.csv`, but it isn't. Have another look."
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        check_file('/home/repl/last.csv').multi(
            has_code('2017-07-17,canine', incorrect_msg=patt%'2017-07-17,canine'),
            has_code('2017-08-13,canine', incorrect_msg=patt%'2017-08-13,canine')
        ),
        test_cmdline([['tail', 'n:', rxc(r'seasonal/winter.csv$'), {'-n': '5'}]],
                     redirect='last.csv',
                     incorrect_msg='Redirect the output of `tail -n 5 seasonal/winter.csv` to `last.csv` with `>`.')
    )
)
Ex().success_msg("Nice! Let's practice some more!")
```


`@possible_answers`


`@feedback`


---

## How can I use a command's output as an input?

```yaml
type: BulletConsoleExercise 
xp: 0 
key: f47d337593   
```


Suppose you want to get lines from the middle of a file.
More specifically,
suppose you want to get lines 3-5 from one of our data files.
You can start by using `head` to get the first 5 lines
and redirect that to a file,
and then use `tail` to select the last 3:

```{shell}
head -n 5 seasonal/winter.csv > top.csv
tail -n 3 top.csv
```

A quick check confirms that this is lines 3-5 of our original file,
because it is the last 3 lines of the first 5.


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

## What's a better way to combine commands?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: b36aea9a1e   
```


Using redirection to combine commands has two drawbacks:

1. It leaves a lot of intermediate files lying around (like `top.csv`).
2. The commands to produce your final result are scattered across several lines of history.

The shell provides another tool that solves both of these problems at once called a **pipe**.
Once again,
start by running `head`:

```{shell}
head -n 5 seasonal/summer.csv
```

Instead of sending `head`'s output to a file,
add a vertical bar and the `tail` command *without* a filename:

```{shell}
head -n 5 seasonal/summer.csv | tail -n 3
```

The pipe symbol tells the shell to use the output of the command on the left
as the input to the command on the right.


`@instructions`
Write a pipeline that uses `cut` to select all of the tooth names from column 2 of `seasonal/summer.csv`
and then `grep -v` to exclude the header line containing the word "Tooth".

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg = 'Have you piped the result of `cut -d , -f 2 seasonal/summer.csv` into `grep -v Tooth` with `|`?')
)
Ex().success_msg("This may be the first time you used `|`, but it's definitely not the last!")
```


`@possible_answers`


`@feedback`


---

## How can I combine many commands?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: b8753881d6   
```


You can chain any number of commands together.
For example,
this command:

```{shell}
cut -d , -f 1 seasonal/spring.csv | grep -v Date | head -n 10
```

will:

1. select the first column from the spring data;
2. remove the header line containing the word "Date"; and
3. select the first 10 lines of actual data.


`@instructions`
In the previous exercise, you used the following command to select all the tooth names from column 2 of `seasonal/summer.csv`:

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

Extend this pipeline with a `head` command to only select the very first tooth name.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
cut -d , -f 2 seasonal/autumn.csv | grep -v Tooth | head -n 1
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    # for some reason has_expr_output with strict=True does not work here...
    has_output('^\s*canine\s*$', incorrect_msg = "Have you used `|` to extend the pipeline with a `head` command? Make sure to set the `-n` flag correctly.")
)
```


`@possible_answers`


`@feedback`


---

## How can I count the records in a file?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: ae6a48d6aa   
```


The command `wc` (short for "word count") prints the number of characters, words, and lines in a file.
You can make it print only one of these using `-c`, `-w`, or `-l` respectively.


`@instructions`
Count how many records in `seasonal/spring.csv` have dates in July 2017. To do this, use `grep` with a partial date to select the lines and pipe this result into `wc` with an appropriate flag to count the lines.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
grep 2017-07 seasonal/spring.csv | wc -l
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg = "Pipe the result of `grep 2017-07 seasonal/spring.csv` into `wc`. Make sure to use the appropriate flag; you want to count the number of _lines_.")
)
```


`@possible_answers`


`@feedback`


---

## How can I specify many files at once?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: 602d47e70c   
```


Most shell commands will work on multiple files if you give them multiple filenames.
For example,
you can get the first column from all of the seasonal data files at once like this:

```{shell}
cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv seasonal/autumn.csv
```

But typing the names of many files over and over is a bad idea:
it wastes time,
and sooner or later you will either leave a file out or repeat a file's name.
To make your life better,
the shell allows you to use **wildcards** to specify a list of files with a single expression.
The most common wildcard is `*`,
which means "match zero or more characters".
Using it,
we can shorten the `cut` command above to this:

```{shell}
cut -d , -f 1 seasonal/*
```

or:

```{shell}
cut -d , -f 1 seasonal/*.csv
```


`@instructions`
Write a single command using `head` to get the first three lines from both `seasonal/spring.csv` and `seasonal/summer.csv`, a total of six lines of data, but *not* from the autumn or winter data files.
Use a wildcard instead of spelling out the files' names in full.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
head -n 3 seasonal/s* # ...or seasonal/s*.csv, or even s*/s*.csv
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg = "You can use `seasonal/s*` to select `seasonal/spring.csv` and `seasonal/summer.csv`. Make sure to only include the first three lines of each file with the `-n` flag!")
)
Ex().success_msg("Good work!")
```


`@possible_answers`


`@feedback`


---

## What other wildcards can I use?

```yaml
type: PureMultipleChoiceExercise 
lang: bash
xp: 50 
key: f8feeacd8c   
```


The shell has other wildcards as well,
though they are less commonly used:

- `?` matches a single character, so `201?.txt` will match `2017.txt` or `2018.txt`, but not `2017-01.txt`.
- `[...]` matches any one of the characters inside the square brackets, so `201[78].txt` matches `2017.txt` or `2018.txt`, but not `2016.txt`.
- `{...}` matches any of the comma-separated patterns inside the curly brackets, so `{*.txt, *.csv}` matches any file whose name ends with `.txt` or `.csv`, but not files whose names end with `.pdf`.

<hr/>

Which expression would match `singh.pdf` and `johel.txt` but *not* `sandhu.pdf` or `sandhu.txt`?


`@instructions`


`@hint`
Match each expression against each filename in turn.

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
- `[sj]*.{.pdf, .txt}`
- `{s*.pdf, j*.txt}`
- `[singh,johel]{*.pdf, *.txt}`
- [`{singh.pdf, j*.txt}`]

`@feedback`
- No: `.pdf` and `.txt` are not filenames.
- No: this will match `sandhu.pdf`.
- No: the expression in square brackets matches only one character, not entire words.
- Correct!

---

## How can I sort lines of text?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: f06d9e310e   
```


As its name suggests,
`sort` puts data in order.
By default it does this in ascending alphabetical order,
but the flags `-n` and `-r` can be used to sort numerically and reverse the order of its output,
while `-b` tells it to ignore leading blanks
and `-f` tells it to **f**old case (i.e., be case-insensitive).
Pipelines often use `grep` to get rid of unwanted records
and then `sort` to put the remaining records in order.


`@instructions`
Remember the combination of `cut` and `grep` to select all the tooth names from column 2 of `seasonal/summer.csv`?

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

Starting from this recipe, sort the names of the teeth in `seasonal/winter.csv` (not `summer.csv`) in descending alphabetical order. To do this, extend the pipeline with a `sort` step.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort -r
```


`@sct`

```{python}
Ex().multi(
    has_cwd('/home/repl'),
    has_expr_output(incorrect_msg = "Have you used `|` to extend the pipeline with a `sort` command? Make sure to work with `seasonal/winter.csv` and to sort in descending alphabetical order with the appropriate flag.")
)
```


`@possible_answers`


`@feedback`


---

## How can I remove duplicate lines?

```yaml
type: ConsoleExercise 
lang: shell
xp: 0 
skills: 1
key: ed77aed337   
```


Another command that is often used with `sort` is `uniq`,
whose job is to remove duplicated lines.
More specifically,
it removes *adjacent* duplicated lines.
If a file contains:

```
2017-07-03
2017-07-03
2017-08-03
2017-08-03
```

then `uniq` will produce:

```
2017-07-03
2017-08-03
```

but if it contains:

```
2017-07-03
2017-08-03
2017-07-03
2017-08-03
```

then `uniq` will print all four lines.
The reason is that `uniq` is built to work with very large files.
In order to remove non-adjacent lines from a file,
it would have to keep the whole file in memory
(or at least,
all the unique lines seen so far).
By only removing adjacent duplicates,
it only has to keep the most recent unique line in memory.


`@instructions`
Write a pipeline to:

- get the second column from `seasonal/winter.csv`,
- remove the word "Tooth" from the output so that only tooth names are displayed,
- sort the output so that all occurrences of a particular tooth name are adjacent; and
- display each tooth name once along with a count of how often it occurs.

The start of your pipeline is the same as the previous exercise:

```
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth
```

Extend it with a `sort` command, and use `uniq -c` to display unique lines with a count of how often each occurs rather than using `uniq` and `wc`.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{shell}
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort | uniq -c
```


`@sct`

```{python}
from shellwhat_ext import test_cmdline, rxc
Ex().multi(
    has_cwd('/home/repl'),
    check_correct(
        has_expr_output(),
        test_cmdline([['cut', 'd:f:', rxc(r'seasonal/winter.csv$'), {'-d': ',', '-f' : '2'}],
                      ['grep', 'v', 'Tooth', {'-v': None}],
                      ['sort', 'r'],
                      ['uniq', 'c']],
                     incorrect_msg="Starting from `cut -d , -f 2 seasonal/winter.csv | grep -v Tooth`, pipe the result into `sort` (without flags) after which you pipe the result into `uniq -c`.")
    )
)
Ex().success_msg("Great! After all of this work on a pipe, it would be nice if we could store the result, no?")
```


`@possible_answers`


`@feedback`


---

## How can I save the output of a pipe?

```yaml
type: MultipleChoiceExercise 
lang: shell
xp: 50 
skills: 1
key: 4115aa25b2   
```


The shell lets us redirect the output of a sequence of piped commands:

```{shell}
cut -d , -f 2 seasonal/*.csv | grep -v Tooth > teeth-only.txt
```

However, `>` must appear at the end of the pipeline:
if we try to use it in the middle, like this:

```{shell}
cut -d , -f 2 seasonal/*.csv > teeth-only.txt | grep -v Tooth
```

then all of the output from `cut` is written to `teeth-only.txt`,
so there is nothing left for `grep`
and it waits forever for some input.

<hr>

What happens if we put redirection at the front of a pipeline as in:

```{shell}
> result.txt head -n 3 seasonal/winter.csv
```


`@instructions`
- [The command's output is redirected to the file as usual.]
- The shell reports it as an error.
- The shell waits for input forever.

`@hint`
Try it out in the shell.

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
Ex().has_chosen(1, ['Correct!', 'No; the shell can actually execute this.', 'No; the shell can actually execute this.'])
```


`@possible_answers`


`@feedback`


---

## How can I stop a running program?

```yaml
type: ConsoleExercise 
xp: 0 
key: d1694dbdcd   
```


The commands and scripts that you have run so far have all executed quickly,
but some tasks will take minutes, hours, or even days to complete.
You may also mistakenly put redirection in the middle of a pipeline,
causing it to hang up.
If you decide that you don't want a program to keep running,
you can type Ctrl-C to end it.
This is often written `^C` in Unix documentation;
note that the 'c' can be lower-case.


`@instructions`
Run the command:

```{shell}
head
```

with no arguments (so that it waits for input that will never come)
and then stop it by typing <kbd>Ctrl</kbd> + <kbd>C</kbd>.

`@hint`


`@pre_exercise_code`

```{shell}

```


`@sample_code`

```{shell}

```


`@solution`

```{bash}
# This solution is different from what you should do.
# Simply type head, hit Enter and exit the running program with Ctrl + C.
echo 'head'
```


`@sct`

```{python}
Ex().has_code(r'\s*head\s*', fixed=False, incorrect_msg="Have you used `head`?")
```


`@possible_answers`


`@feedback`


---

## Wrapping up

```yaml
type: BulletConsoleExercise 
xp: 0 
key: 659d3caa48   
```


To wrap up,
you will build a pipeline to find out how many records are in the shortest of the seasonal data files.


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

