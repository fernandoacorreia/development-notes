# Git

## git bisect

Git bisect example:

```
git bisect start
git bisect bad HEAD
git bisect good HEAD~9
git bisect run ./test_script.sh
```

## git log -L

git log -L :func_name:path/to/file.c lets you view the history (diff evolution) of a specific function, but Git needs to understand where functions start and end in that file.

Git determines this using “hunk header” regular expressions — these patterns are defined in .gitattributes.

Example .gitattributes:

```
# C / C++
*.c diff=cpp
*.h diff=cpp
*.cpp diff=cpp
*.hpp diff=cpp

# Python
*.py diff=python

# Shell scripts
*.sh diff=sh

# JavaScript
*.js diff=java

# Go
*.go diff=golang
```

## git log -S

git log -S<string> helps you find commits where a specific string was added or removed in the code.

In other words, it searches through the diffs (changes) of each commit, not just the commit messages, to locate where that string appeared or disappeared in the repository’s history.

Example:

```
git log -S"my_function"
```

Use -p to show the actual patch:

```
git log -S"my_function" -p
```
