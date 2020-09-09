---
title: "Mastering Vim Part 1: The Basics"
date: 2019-09-10
tags:
  - vim 
layout: layouts/post.njk
---

If you have been programming for a while chances are that you have heard about vim. Maybe you tried it and didn't find it useful. Neither did I, but after a while I began to notice the difference.

It is light-weight, which means that it opens files very quickly. Other editors have trouble opening large files and take noticeable amount of time to open. So, I switched back to vim and never looked back.

Before you get into vim, it is recommended that you **learn touch typing first**. You can still continue to read the article, but to be honest, you are not going to have a good time if you are always looking at the keyboard.

## Installing

If you are using Mac OS or Linux, chances are that you already have it in your system. Open terminal application and type `vim` to check if you have it. If not, you can install it with a few commands.

### Linux

On Debian based systems

```text
sudo apt install vim
```

If you use other package manager like Pacman or something, then you know how to install it. You can also download it from the [official website](https://www.vim.org/download.php).

### Mac

You can use [homebrew](https://brew.sh) to install it on Mac

```text
brew install vim
```

### Windows

For windows you have to install the GUI version of vim (gVim) if you want least amount headaches. Download the installer from [here](https://www.vim.org/download.php#pc)

## The Basics

Alright, now that you have installed vim, let's jump right into it. First, open vim by typing `vim` in the terminal if you are on Mac or Linux. On Windows, double click the gVim application to launch it.

You will see an ugly looking black screen. Don't worry about it, we are going to make it look way better in the next tutorial. For now, bear with me.

### Modes

How do you move around when you can't use mouse at all? Well the answer is using something called modes. There are three modes in Vim:

- `NORMAL` - To move around the text and execute some commands like you would do using a mouse.
- `INSERT` - To insert text into the file.
- `VISUAL` - To select the text and execute commands on that selected text.

Normal mode is the mode in which you are going to spend most of your time in. Moving around and debugging code is really fast in this mode. To enter normal mode you press `ESC` key. You will notice that you cannot enter text in this mode if you try to. **To switch to other modes you have to be in normal mode first**. Always.

To go into insert mode you click `i` (it is case sensitive). The last line on your terminal will say `-- INSERT --`. Now you can enter as much text as you want and click `ESC` when you are done with inserting. Always use normal mode to move around in text. **Please don't use arrow keys** or there is no point in using vim.

To go into visual mode you click `v`, `V` or `Ctrl+v` depending on what you want to do. For, now don't worry about it. Just know that it is used to select text. I will get into visual mode separately. When you press either of those key bindings, the last line on your terminal will say `-- VISUAL --`. Most of the key bindings for visual mode is same as normal mode.

### Navigation

Okay, now let's get into it. **All of the key binding from now on have to be written in normal mode** unless stated otherwise. To move around in the text you need to use `h`, `j`, `k`, `l`. These are your arrow keys from now on. `h` is used to go left, `j` for down, `k` for up, `l` for right like so:

```text
        ^
        k
<h             l>
        j
        v
```

**It is going to be hard at first. But, the payoff is going to be huge**. You are going to mess up a lot. Don't worry, you will get there. Enter a bunch of text using insert mode and try this.

Now some other key bindings for moving around:

- `w` - Go forward one **w**ord.
- `b` - Go **b**ackward one word.
- `e` - Go to the **e**nd of the word.
- `^` or `0` - Go to the beginning of the line.
- `$` - Go to the end of the line.
- `G` - Go to the end of the file.
- `gg` - Go to the beginning of the file

End of the word is marked by a space. End of the line is marked by new line character. Now try these and get a hang of it. The first three of this list are used most to quickly move around once you are on the correct line.

This is all you are going to need to be efficient at moving around the code. If you are too lazy you can try this [interactive vim](https://www.openvim.com) tutorial or an [adventure game](https://vim-adventures.com) to learn these key bindings.

### Editing

Now that you know your way around text, let's look into editing text:

- `a` - Insert after the current character.
- `A` - Insert at the end of the line.
- `x` - Cut current letter.
- `cw` - **C**hange **w**ord from the current character to the end of the word.
- `dw` - **D**elete **w**ord from the current character to the end of the word.
- `dd` - Cut the current line.
- `yy` - Copy the current line.
- `p` - **P**aste the last cut/copied text after.
- `P` - Paste the last cut/copied text before.
- `o` - Insert new line after.
- `O` - Insert new line before.

The difference between `cw` and `dw` is that `cw` puts you into insert mode, while `dw` does not. `A`, `p`, `yy`, `o` and `dd` are used the most, so learn them first.

### Repetition

Let me show you something cool. You can use numbers to extend the functionality of these commands. For example, typing `5` before `w` will move you 5 words ahead. Similarly, `10j` will move the cursor 10 lines down. For keys with two keystrokes like `dd` and `yy` you use this feature differently. You have to use these commands like `d4d` and `y4y`.

Of course, keys like `A` and `0` do not support this feature because of obvious reasons, but most of the keys that you use will support this feature.

Congratulations, now you know about 70% of what you are going to use all the time.

### Searching (and Replacing)

Alright, for searching text there are two methods `f` and `/`:

- You use `f` to search next occurrence of a character. For example, if you type `ft` the cursor will move to the next occurrence of "t" in the text. To search this way but backwards, you type `F` instead of `f`.
- For searching a sub-string all over the text you use `/` followed by the text you want to search and then pressing enter. For example, to search for "hello world" in the text, you type `/hello world` and press enter. To find the **n**ext occurrence you type `n`, for previous, you type `N` after pressing enter. This is more like the search feature in your common text editors and IDEs.
- Replacing text a bit more complicated. You start by typing `:`, then the range of lines you would like to search. For example, `23, 53` or `%` to search whole file. Then you type `s`, followed by `/<text-to-search>`, `/<text-to-replace-with>` and then some options. Let me give you some examples:
  - `:%s/foo/bar/gc`- Searches the whole file for every occurrence (`g`) of `foo`, replaces it with `bar`. Asks for permission (`c`)
  - `:3,10 s/foo/bar`- Searches from line 3 to line 10 for next occurrence of `foo`, replaces it with `bar` without asking permission

This might look cumbersome at first, but believe me, if you stick with it you are going to be jumping around text faster than you ever did with a mouse.

### Visual mode

As said earlier, to select text you use visual mode. There are three key bindings to enter this mode:

- `v`- Character mode
- `V`- Line mode
- `Ctrl+v`- Block mode

In Character mode you can select text letter by letter. Press `v` and see for yourself. When you move around using the navigation keys, the text your cursor passed gets selected. It is very similar to how you select stuff in a paragraph of text.

In Line mode you can select the whole line instead of a letter. When you move down or up, the lines the cursor passes get selected. Moving left or right does not affect the selection.

Now, Block mode is something different. Think of it like drawing a rectangle in Microsoft Paint using rectangle tool. The point at which you enter Block mode becomes the top left corner of the rectangle and the cursor becomes the bottom right corner.

To perform actions on the selected text, you simply use the key bindings of copying, cutting just like normal mode.

### Saving, Quitting and more

There are different commands to save and/or quit in Vim. Don't worry they are very straightforward:

- `:w` - **W**rite changes to file
- `:w <filename>` - **W**rite changes to a file named `<filename>` in current working directory
- `:q!` - Quit without saving

In Vim you can write some special commands using `:`. There are many more commands that you can execute to perform some of the important tasks. Writing ,quitting and search/replace are some of them. Commands are also used to install and launch plugins, which I will go over in the [next post](/blog/mastering-vim-part-2-vimrc-and-plugins).

Just like other commands in vim. `w` and `q` can be composed into one command like so:

- `:wq` - **W**rite and **Q**uit
- `:wq <filename>` - **W**rite and **Q**uit to a file named `<filename>` in current working directory

If you are familiar with command line or \*nix shell, then some of these might be familiar to you

- `:cd` - Change working directory
- `:pwd` - Print the current working directory
- `:e <path-to-file>` - Opens the file to be edited. (Tab completion is enabled)

You can also execute shell commands with `:! <command>`

## Still too much?

Learning vim can be hard, it is going to be uncomfortable. To really learn it you have to **use it for your day to day tasks**. Replace your text editor with Vim and do all your coding and text editing with it. It will not take more than a week to become really fast with it and you will never want to use mouse again.

Use `vimtutor`, an interactive tutorial about Vim which gets installed when you install Vim. On Windows you can find it in start menu. On Linux and Mac just type `vimtutor` in terminal. It is a great way to practice vim if you forget how some key bindings.

If you don't want to install vim but want to try it out, many editors and IDEs have plugins that enable vim keybindings. You can start there and gradually move on to the real thing.

To look at all the key bindings and commands that you can use [vimhelp](https://vimhelp.org), it is an amazing reference.

In the [next part](/blog/mastering-vim-part-2-vimrc-and-plugins) of this series I will show you how to make your vim look something like this.

![Modified Vim](/img/vim-modified.png "Modified Vim")
