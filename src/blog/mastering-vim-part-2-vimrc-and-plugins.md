---
title: "Mastering Vim Part 2: Vimrc and Plugins"
date: 2019-09-14
tags:
  - vim
layout: layouts/post.njk
---

In the last post I showed you basic key bindings and commands to get you started with Vim. In this one, I am going to show you how can you make Vim look like the way you want. This is what my configuration looks like at the time of writing.

![Modified Vim](/img/vim-modified.png "Modified Vim")

Vim uses a file named `.vimrc` (or `_vimrc` on Windows) which is used for customizing your experience. It is also used to load any plugins and themes which enhance your workflow.

## The vimrc file

To configure Vim, you put some commands in a special file named `.vimrc`. It is located in the home directory. For windows, you can find your home directory by executing `:echo $HOME`.

Here is a snippet of my vimrc file:

```text
set path+=**
set nocompatible
set encoding=utf-8
set number relativenumber
set linebreak
set showmatch
set novisualbell
set hlsearch
set smartcase
set ignorecase
set incsearch
set autoindent
set cindent
set shiftwidth=4
set tabstop=4
set smartindent
set expandtab
set ruler
set undolevels=1000
set backspace=indent,eol,start
set nohlsearch
set colorcolumn=80
```

I am not going to explain what all of these commands do but here are a few:

- `set number relativenumber` - It enables line numbering in files that you edit with vim. `relativenumber` just numbers the line relative to current cursor position. It helps with `dd` and `yy` commands when you have to cut/copy a few lines, or move around in file.
- `set colorcolumn=80` - It highlights the eightieth column.

You can find all the possible options using `:options` command. Also, you can search for other people's vimrc files on GitHub. Most of them won't have any problem with you copying (ask them first though). You can find mine [here](https://github.com/aditya-azad/configs/). **I encourage you to modify this as per your own needs**.

## Plugins

Plugins are a great way to extend the functionality of vim and make it more IDE-like. Keep in mind that Vim is not an IDE and you should not think of it in that way. It is just a simple text editor similar to your good ol' notepad but way better.

There are many ways to install plugins in vim, but the easiest way is to use a package manager. If you have worked with JavaScript or Ruby then you might be familiar with npm and gem. Vim has similar package managers like [Vundle](https://github.com/VundleVim/Vundle.vim) and [pathogen](https://github.com/tpope/vim-pathogen), but I personally use [vim-plug](https://github.com/junegunn/vim-plug).

### Installing vim-plug and plugins

To install plugins and store its files you have to create a directory named `.vim` in your vim home (along with `.vimrc` file). This is where vim-plug resides too. Here is how you can install vim-plug:

- Create a directory named `autoload` in `.vim` folder
- Go to [this](https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim) link and copy all the code.
- Make a file named `plug.vim` in `autoload` directory and paste all the code that you copied.
- Put this code into your `.vimrc`

```text
call plug#begin()
Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }
Plug 'itchyny/lightline.vim'
Plug 'scrooloose/nerdtree', { 'on': 'NERDTreeToggle' }
Plug 'morhetz/gruvbox'
call plug#end()
```

- Open vim and execute `:PlugInstall` command

You will notice that vim-plug begins installing the plugins. You can install any plugin that supports vim-plug by only adding `Plug <plugin>` into your `.vimrc` and calling `:PlugInstall`. A good website to find plugins is [Vim Awesome](https://vimawesome.com/). If you have any problems or you do not like vim-plug, you can use some other package manager instead.

Here is the breakdown of the plugins I use:

- [Fzf](https://github.com/junegunn/fzf) - It is a fuzzy-finder used to quickly open files in Vim
- [Lightline](https://github.com/itchyny/lightline.vim) - It is the fancy status bar for vim with is very customizable
- [Nerdtree](https://github.com/scrooloose/nerdtree) - It is a file explorer similar to what you have in other IDEs
- [Gruvbox](https://github.com/morhetz/gruvbox) - It is the awesome theme that makes Vim look pretty

To run Nerdtree you have to run `:NERDTreeToggle` every time. To make it easier to access it I have bounded my `Ctrl-o` to this command. Similarly I have bounded `:FZF` to `;` key.

If you just want the same binding I have, copy [my .vimrc](https://github.com/aditya-azad/configs) file.

## Sidenote

To install Fzf you need to install some dependencies. Just execute `:FZF` command and it will tell you what is missing. To enable Gruvbox theme, you have to have `colorscheme gruvbox` in your `.vimrc`.
