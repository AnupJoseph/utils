---
title: All Bash tricks
description: A starter template to build a bumpchart in Python
slug: bash-tricks
date: 23-03-2024 00:00:00+0000
draft: false
categories:
    - Shell
tags:
    - shell
    - bash

---



<a id="org3da7283"></a>

# Shell tricks

- [Shell tricks](#shell-tricks)
  - [Switch to previous directory](#switch-to-previous-directory)
    - [git](#git)
- [master](#master)
    - [cd](#cd)
  - [Get global ip](#get-global-ip)
  - [Simple commands](#simple-commands)
  - [Loop](#loop)
  - [Loop with specified increment each iteration](#loop-with-specified-increment-each-iteration)
  - [Sequences of letters or numbers](#sequences-of-letters-or-numbers)
  - [Reuse arguments](#reuse-arguments)
  - [Reuse commands](#reuse-commands)
  - [Compare output of two commands](#compare-output-of-two-commands)
  - [Fix last command](#fix-last-command)
  - [Accept interactive commands](#accept-interactive-commands)
  - [Last exit code](#last-exit-code)
  - [Easy backup](#easy-backup)
  - [Print to stderr](#print-to-stderr)
  - [Debugging](#debugging)
  - [Useful `readline` tricks](#useful-readline-tricks)
  - [Repeat command](#repeat-command)
  - [Zipping and Unzip](#zipping-and-unzip)
  - [Find Magic](#find-magic)


<a id="switch-to-previous-directory"></a>

## Switch to previous directory

Switch between the current and previous branch / directory.


<a id="git"></a>

### git

\#+begin<sub>src</sub> sh
$ git branch


<a id="org3233289"></a>

# master

development

$ git checkout development
Switched to branch &rsquo;development&rsquo;
$ git checkout - # Switch to previous
Switched to branch &rsquo;master&rsquo;
$ git checkout -
Switched to branch &rsquo;development&rsquo;
\#+end<sub>src</sub>


<a id="cd"></a>

### cd

    $ pwd
    /
    $ cd /tmp
    $ cd - # Switch to previous
    /
    $ cd -
    /tmp


<a id="get-global-ip"></a>

## Get global ip

    $ curl ifconfig.co # IPv4
    50.110.14.21
    $ curl -6 ifconfig.co # IPv6
    2010:3f3f:113f:0:ea57:4497:7291:e422


<a id="simple-commands"></a>

## Simple commands

Create a script which calls functions by its\` first argument. This is
very useful to create simple scripts which could be a wrapper for other
commands.

    #!/usr/bin/env bash

    function do_this () { echo "call do_this function"; }

    function do_sth() { echo "call do_sth function" }

    case "$1" in
        do_this|do_sth) "$1" ;;
    esac

Execute it:

    $ ./simple-commands.sh do_this
    call do_this function


<a id="loop"></a>

## Loop

Write simple one liner loops if you need to do some batch tasks.

    $ for i in {1..10}; do echo "$i"; done

    # List disk usage by directories
    $ for file in */ .*/ ; do du -sh $file; done


<a id="loop-with-specified-increment-each-iteration"></a>

## Loop with specified increment each iteration

    for i in {1..100..2}; do echo $i; done
    1
    3
    5
    7
    ...


<a id="sequences-of-letters-or-numbers"></a>

## Sequences of letters or numbers

Brace expansion is great for lots of things.

    $ touch file{a..c}
    $ ls
    $ command ls
    filea fileb filec

    $ touch file-{1..15}
    $ ls
    file-1  file-10 file-11 file-12 file-13 file-14 file-15 file-2  file-3  file-4  file-5  file-6  file-7  file-8  file-9
    $ ls file-{9..12}
    file-10 file-11 file-12 file-9

    $ printf "%s\n" file-{a..c}{1..3}
    file-a1
    file-a2
    file-a3
    file-b1
    file-b2
    file-b3
    file-c1
    file-c2
    file-c3

(If you give `printf` more arguments than it expects, it automatically
loops.)


<a id="reuse-arguments"></a>

## Reuse arguments

    $ ls /tmp
    some_file.txt some_archive.tar.gz
    $ cd !$
    /tmp


<a id="reuse-commands"></a>

## Reuse commands

    $ echo "reuse me"
    reuse me
    $ !!
    echo "reuse me"
    reuse me


<a id="compare-output-of-two-commands"></a>

## Compare output of two commands

    diff <(echo "1 2 4") <(echo "1 2 3 4")
    1c1
    < 1 2 4
    ---
     > 1 2 3 4


<a id="fix-last-command"></a>

## Fix last command

    $ ehco foo bar bar
    bash: ehco: command not found
    $ ^ehco^echo
    foo bar baz


<a id="accept-interactive-commands"></a>

## Accept interactive commands

    $ yes | ./interactive-command.sh
    Are you sure (y/n)
    Accepted
    yes: standard output: Broken pipe

The error message is printed because `yes` gets killed by `SIGPIPE`
signal. This happens if the pipe to `./interactive-command.sh` gets
closed but `yes` still wants to write into it.

Ignore error message:

`$ yes 2>/dev/null | ./interactive-command.sh`


<a id="last-exit-code"></a>

## Last exit code

    $ ls /tmp
    some_file.txt
    $ echo $?
    0


<a id="easy-backup"></a>

## Easy backup

    $ cp file.txt{,.bak}
    $ ls -1
    file.txt
    file.txt.bak


<a id="print-to-stderr"></a>

## Print to stderr

    $ >&2 echo hello
    hello


<a id="debugging"></a>

## Debugging

Add `-xv` to your bash scripts, i.e.:

    /usr/bin/env bash
    set -xv

or `/bin/bash -xv script.sh`


<a id="useful-readline-tricks"></a>

## Useful `readline` tricks

If you use the standard `bash` `readline` bindings.

-   `C-a` (aka `CTRL+A`) move cursor to beginning of line
-   `C-e` (aka `CTRL+E`) move cursor to end of line
-   `M-.` (aka `ALT+.`) insert last argument of previous command (like
    `!$`, but you can edit it)


<a id="repeat-command"></a>

## Repeat command

Execute a command every two seconds and monitor its\` output. This is
especially useful for waiting until a deployment or infrastructure
provisioning is completed, i.e. on aws.

`watch -n2 echo hello` \` ## Substrings

    $ a="apple orange"

    $ echo ${a#* }
    orange
    $ echo ${a#*p}
    ple orange
    $ echo ${a##*p}
    le orange

    $ echo ${a% *}
    apple
    $ echo ${a%p*}
    ap
    $ echo ${a%%p*}
    a

The # for finding first occurence from the start and % for the first
occurence from the end. \* for matching any pattern. For greedy matching
\## and %%.


<a id="zipping-and-unzip"></a>

## Zipping and Unzip

To zip a folder with certain file types excluded

    zip -x {file_types} -r archive.zip {folder}


## Find Magic
All find tricks are present [here](/find)