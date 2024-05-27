---
title: "Tips&Tricks in Linux CLI I widly use, part 1"
date: 2023-08-08T22:03:00+03:00
draft: true
---

Stop! Do not go away! I know that there are a really huge number of useful CLI commands in Linux and one more article about it it's too boring to read. And a lot of articles just rewrite a big list of commands you never seen and never would use. But this article is different! I use this commands almost every time I working in Linux CLI. So, here we go.

## Shell

At first, I use [ZSH](https://ru.wikipedia.org/wiki/Zsh) shell instead of default Bash, with [oh-my-zsh](https://ohmyz.sh/) extension. It has pretty view and number of themes. Here is how looks my CLI now:
```bash
(leks.us) ➜  /Users/andrey/sites/leksus/leks.us git:(master)
```
The first path inside parentheses is current python virtual environment (yes, I create it for this site just for demonstrate, because this site is markdown based static site). Next,  there is a path to working (current) directory. And the last path is shown that this directory is a git directory (containing `.git` folder) and it is tracked. And  I'm in `master` branch now.

This view is very useful and I don't spent time to understand where am I and what environment surrounded me.

## Autocomplete, history, history-based-completion

One press `tab` button autocompletes program I want to start. 
Double `tab` button show me the possible file and folder paths I can hand over to that program.

Or, if I run this program with some params before, I can just press up arrow and it will filled up.

Or maybe you want to run a command with long string you run before,  but don't remember exactly how it looks like. Just search in history! How it was? Something with `docker exec`? No problem:
```bash
history | grep 'docker exec'
```

## Shortcuts

##### `command+delete` to delete whole string in CLI
Sometimes you write command in the shell, but suddenly understand, that this is not that you want. You need to clean this string, delete it. Of course, you can press `delete` a lot of times, ha-ha. But with `command+delete` you will delete all the string in one second!

##### `control+R` - search in history by shortcuts
Most of the shells has command to search n history. Just press `control+R` and you will see interactive input field. Every symbol you write runs history search, and the last result goes to command line.

##### `command+left-right arrows` to move to home/end in CLI
It's obvious, but maybe someone didn't know it.

##### `option++left-right arrows`
This is less-known feature, by passing `option+arrow` you can jump by words in your CLI string. 

Next I will write about useful commands in Linux. Don't worry, it is really useful, no bullshit! :)

