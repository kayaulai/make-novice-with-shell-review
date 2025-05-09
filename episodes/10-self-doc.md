---
title: "BONUS: Self-Documenting Makefiles"
teaching: 10
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Write self-documenting Makefiles with built-in help.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How should I document a Makefile?

::::::::::::::::::::::::::::::::::::::::::::::::::

Many bash commands, and programs that people have written that can be
run from within bash, support a `--help` flag to display more
information on how to use the commands or programs. In this spirit, it
can be useful, both for ourselves and for others, to provide a `help`
target in our Makefiles. This can provide a summary of the names of
the key targets and what they do, so we don't need to look at the
Makefile itself unless we want to. For our Makefile, running a `help`
target might print:

```bash
$ make help
```

```output
results.txt : Generate Zipf summary table.
dats        : Count words in text files.
clean       : Remove auto-generated files.
```

So, how would we implement this? We could write a rule like:

```make
.PHONY : help
help :
	@echo "results.txt : Generate Zipf summary table."
	@echo "dats        : Count words in text files."
	@echo "clean       : Remove auto-generated files."
```

But every time we add or remove a rule, or change the description of a
rule, we would have to update this rule too. It would be better if we
could keep the descriptions of the rules by the rules themselves and
extract these descriptions automatically.

The bash shell can help us here. It provides a command called
[sed][sed-docs] which stands for 'stream editor'. `sed` reads in some
text, does some filtering, and writes out the filtered text.

So, we could write comments for our rules, and mark them up in a way
which `sed` can detect. Since Make uses `#` for comments, we can use
`##` for comments that describe what a rule does and that we want
`sed` to detect. For example:

```make
## results.txt : Generate Zipf summary table.
results.txt : $(ZIPF_SRC) $(DAT_FILES)
	$(LANGUAGE) $^ > $@

## dats        : Count words in text files.
.PHONY : dats
dats : $(DAT_FILES)

%.dat : $(COUNT_SRC) books/%.txt
	$(LANGUAGE) $^ $@

## clean       : Remove auto-generated files.
.PHONY : clean
clean :
	rm -f $(DAT_FILES)
	rm -f results.txt

## variables   : Print variables.
.PHONY : variables
variables:
	@echo TXT_FILES: $(TXT_FILES)
	@echo DAT_FILES: $(DAT_FILES)
```

We use `##` so we can distinguish between comments that we want `sed`
to automatically filter, and other comments that may describe what
other rules do, or that describe variables.

We can then write a `help` target that applies `sed` to our `Makefile`:

```make
.PHONY : help
help : Makefile
	@sed -n 's/^##//p' $<
```

This rule depends upon the Makefile itself. It runs `sed` on the first
dependency of the rule, which is our Makefile, and tells `sed` to get
all the lines that begin with `##`, which `sed` then prints for us.

If we now run

```bash
$ make help
```

we get:

```output
 results.txt : Generate Zipf summary table.
 dats        : Count words in text files.
 clean       : Remove auto-generated files.
 variables   : Print variables.
```

If we add, change or remove a target or rule, we now only need to
remember to add, update or remove a comment next to the rule. So long
as we respect our convention of using `##` for such comments, then our
`help` rule will take care of detecting these comments and printing
them for us.

:::::::::::::::::::::::::::::::::::::::::  callout

## Where We Are

[This Makefile](files/code/08-self-doc/Makefile)
and [its accompanying `config.mk`](files/code/08-self-doc/config.mk)
contain all of our work so far.


::::::::::::::::::::::::::::::::::::::::::::::::::

[sed-docs]: https://www.gnu.org/software/sed/


:::::::::::::::::::::::::::::::::::::::: keypoints

- Document Makefiles by adding specially-formatted comments and a target to extract and format them.

::::::::::::::::::::::::::::::::::::::::::::::::::


