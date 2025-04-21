---
title: Introduction
teaching: 15
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain what Make is for.
- Explain why Make differs from shell scripts.
- Name other popular build tools.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I make my results easier to reproduce?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Pipelining with Make

Together these scripts we just saw implement a common workflow:

1. Read a data file.
2. Perform an analysis on this data file.
3. Write the analysis results to a new file.
4. Plot a graph of the analysis results.
5. Save the graph as an image, so we can put it in a paper.
6. Make a summary table of the analyses

Running `countwords.py` and `plotcounts.py` at the shell prompt, as we
have been doing, is fine for one or two files. If, however, we had 5
or 10 or 20 text files,
or if the number of steps in the pipeline were to expand, this could turn into
a lot of work.
Plus, no one wants to sit and wait for a command to finish, even just for 30
seconds.

The most common solution to the tedium of data processing is to write
a shell script that runs the whole pipeline from start to finish.

So to reproduce the tasks that we have just done we create a new file
named `run_pipeline.sh` in which we place the commands one by one.
Using a text editor of your choice (e.g. for nano use the command `nano run_pipeline.sh`) copy and paste the following text and save it.

```bash
# USAGE: bash run_pipeline.sh
# to produce plots for isles and abyss
# and the summary table for the Zipf's law tests

python countwords.py books/isles.txt isles.dat
python countwords.py books/abyss.txt abyss.dat

python plotcounts.py isles.dat isles.png
python plotcounts.py abyss.dat abyss.png

# Generate summary table
python testzipf.py abyss.dat isles.dat > results.txt
```

Run the script and check that the output is the same as before:

```bash
$ bash run_pipeline.sh
$ cat results.txt
```

This shell script solves several problems in computational reproducibility:

1. It explicitly documents our pipeline,
  making communication with colleagues (and our future selves) more efficient.
2. It allows us to type a single command, `bash run_pipeline.sh`, to
  reproduce the full analysis.
3. It prevents us from *repeating* typos or mistakes.
  You might not get it right the first time, but once you fix something
  it'll stay fixed.

Despite these benefits it has a few shortcomings.

Let's adjust the width of the bars in our plot produced by `plotcounts.py`.

Edit `plotcounts.py` so that the bars are 0.8 units wide instead of 1 unit.
(Hint: replace `width = 1.0` with `width = 0.8` in the definition of
`plot_word_counts`.)

Now we want to recreate our figures.
We *could* just `bash run_pipeline.sh` again.
That would work, but it could also be a big pain if counting words takes
more than a few seconds.
The word counting routine hasn't changed; we shouldn't need to recreate
those files.

Alternatively, we could manually rerun the plotting for each word-count file.
(Experienced shell scripters can make this easier on themselves using a
for-loop.)

```bash
for book in abyss isles; do
    python plotcounts.py $book.dat $book.png
done
```

With this approach, however,
we don't get many of the benefits of having a shell script in the first place.

Another popular option is to comment out a subset of the lines in
`run_pipeline.sh`:

```bash
# USAGE: bash run_pipeline.sh
# to produce plots for isles and abyss
# and the summary table for the Zipf's law tests.

# These lines are commented out because they don't need to be rerun.
#python countwords.py books/isles.txt isles.dat
#python countwords.py books/abyss.txt abyss.dat

python plotcounts.py isles.dat isles.png
python plotcounts.py abyss.dat abyss.png

# Generate summary table
# This line is also commented out because it doesn't need to be rerun.
#python testzipf.py abyss.dat isles.dat > results.txt
```

Then, we would run our modified shell script using `bash run_pipeline.sh`.

But commenting out these lines, and subsequently uncommenting them,
can be a hassle and source of errors in complicated pipelines.

What we really want is an executable *description* of our pipeline that
allows software to do the tricky part for us:
figuring out what steps need to be rerun.

For our pipeline Make can execute the commands needed to run our
analysis and plot our results. Like shell scripts it allows us to
execute complex sequences of commands via a single shell
command. Unlike shell scripts it explicitly records the dependencies
between files - what files are needed to create what other files -
and so can determine when to recreate our data files or
image files, if our text files change. Make can be used for any
commands that follow the general pattern of processing files to create
new files, for example:

- Run analysis scripts on raw data files to get data files that
  summarize the raw data (e.g. creating files with word counts from book text).
- Run visualization scripts on data files to produce plots
  (e.g. creating images of word counts).
- Parse and combine text files and plots to create papers.
- Compile source code into executable programs or libraries.

There are now many build tools available, for example [Apache
ANT][apache-ant], [doit], and [nmake] for Windows.
Which is best for you depends on your requirements,
intended usage, and operating system. However, they all share the same
fundamental concepts as Make.

Also, you might come across build generation scripts e.g. [GNU
Autoconf][autoconf] and [CMake][cmake].  Those tools do not run the
pipelines directly, but rather generate files for use with the build
tools.

:::::::::::::::::::::::::::::::::::::::::  callout

## Why Use Make if it is Almost 40 Years Old?

Make development was started by Stuart Feldman in 1977 as a Bell
Labs summer intern. Since then it has been undergoing an active
development and several implementations are available. Since it
solves a common issue of workflow management, it remains in
widespread use even today.

Researchers working with legacy codes in C or FORTRAN, which are
very common in high-performance computing, will, very likely
encounter Make.

Researchers can use Make for implementing reproducible
research workflows, automating data analysis and visualisation
(using Python or R) and combining tables and plots with text to
produce reports and papers for publication.

Make's fundamental concepts are common across build tools.


::::::::::::::::::::::::::::::::::::::::::::::::::

[GNU Make][gnu-make] is a free-libre, fast, [well-documented][gnu-make-documentation],
and very popular Make implementation. From now on, we will focus on it, and when we say
Make, we mean GNU Make.

[zipfs-law]: https://en.wikipedia.org/wiki/Zipf%27s_law
[apache-ant]: https://ant.apache.org/
[doit]: https://pydoit.org/
[nmake]: https://docs.microsoft.com/en-us/cpp/build/reference/nmake-reference
[autoconf]: https://www.gnu.org/software/autoconf/autoconf.html
[cmake]: https://www.cmake.org/
[gnu-make]: https://www.gnu.org/software/make/
[gnu-make-documentation]: https://www.gnu.org/software/make/#documentation


:::::::::::::::::::::::::::::::::::::::: keypoints

- Make allows us to specify what depends on what and how to update things that are out of date.

::::::::::::::::::::::::::::::::::::::::::::::::::