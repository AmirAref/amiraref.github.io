+++
title = "Tracking dotfiles, What is it and Why ?"
date = "2024-05-14"
description = "a beginner guide for keep and traking dotfiles"
[taxonomies]
tags = ["linux", "unix"]
[extra]
cover_image = "images/dotfiles.png"
author = "Amir Hasan Aref Asl"
+++

# What are the dotfiles ?
Hi, in this article i want to explain and talk about the **dotfiles**.
The first question is what are the dotfiles ?
it's too clear, the files that their names starts with a dot `.` which (in most of operating systems) is a **hidden** file.

# Why the dotfiles are important ?
If you are a user of a Unix-like operating system (like GNU/LINUX distributions, mac-os, etc), the dotfiles is too important for you.
Most of the applications use their specific dotfile to store their your configuration.  
for example have you ever used the **vim** editor ? The vim editor load it's configuration from the `.vimrc` file from you home directory (`~/.vimrc`).
If you explore on your home directory and the `~/.config/` directory, you will probably see many of the dotfiles of different software (see hidden files with command : `ls -a`) such as `.bashrc`, `.zshrc`, `.gitconfig`, etc.

## Benefits of Tracking the dotfiles
after a while that you have been struggled with many of software's configuration and make them customize for yourself (specially in Linux) you probably trying to **keep** your dotfiles somewhere to use them in the future or to use in the other machines.  
so after you save your specific dotfiles, you just need to copy them in your new machine and enjoy working with same configuration and customization that you had in the other machine.

# How Keep and track the dotfiles ?
One of the best ways to store the dotfiles is using the **Git**, but managing your dotfiles using git is a little different that tracking your project's source code.  
in the past and until recently, i was using an old method to store my dotfiles with git that think it was not too optimized. In this old method you have to create a `bare` repository and use the alias.  
i don't want to explain this method here and my reason for that is that in my experience i found this method is not too optimized with **shell ta autocompletion** and at most of the times i had problem with that, but if you interested in, you can find it with the **_"bare repository and alias method"_** in links that i will put below.


## Track your dotfiles using the stow
there is useful program named **[stow](https://www.gnu.org/software/stow/)** which is a symlink farm manager. The stow has many usages but it can provide a structured approach to store your dotfiles.

1. first make sure the **stow** is installed on your machine, in most of Linux distros it's already installed, but if not, install it using your package manager.
2. first of all you should create a directory to store your dotfiles in your **home directory**, in this case we name it `.dotfiles`.
```bash
mkdir ~/.dotfiles
cd ~/.dotfiles
```
3. next you should create your dotfiles in subdirectory of this directory (or move them to it).
Let's imagine we have `~/.vimrc` file in the home directory, you should move it to a new folder on `.dotfiles` directory :
```bash
mkdir vim
mv ~/.vimrc ~/.dotfiles/vim/
```

{% admonition(type="tip", title="Tip") %}
the name of subdirectory can be anything you want, but it's better to use a related name to make the files structure more clear.
{% end %}

You should consider that the **stow** will follow the same folder structure that is placed in home directory.  
For example the neovim's config folder that placed in `~/.config/nvim/` would be like : `~/.dotfiles/nvim/.config/nvim/`.  
Do the same thing for all of your dotfiles.  

Here is the structure of my `.dotfiles` directory ([my dotfiles repository](https://github.com/AmirAref/dotfiles/)):
```
.dotfiles/
├── fish
│   └── .config
│       └── fish
│           ├── config.fish
├── git
│   ├── .gitconfig
├── kitty
│   └── .config
│       └── kitty
│           └── kitty.conf
├── nvim
│   └── .config
│       └── nvim
│           ├── .gitignore
│           ├── init.lua
│           ├── lazy-lock.json
│           ├── lua
│           ├── .stylua.toml
│           └── TODO.md
├── tmux
│   ├── .tmux
│   │   └── plugins
│   │       └── tpm
│   └── .tmux.conf
├── vim
│   ├── .vim
│   │   └── coc-settings.json
│   └── .vimrc
└── yt-dlp
    └── .config
        └── yt-dlp
            └── config
```
4. after this, run the following command to make symlinks using stow (put your own folders' names) :
```bash
stow git vim nvim kitty
```
now if you go to your home directory, you'll see the **symlinks** have been created :
```bash
➜ ls -al .vimrc
20 May  6 00:32 .vimrc -> .dotfiles/vim/.vimrc
```

5. at the end, initialize git in your `.dotfiles` folder, then create a commit and push it to your online git repository :
```bash
cd ~/.dotfiles/
git init
git commit -m "Initial Commit"
git push origin master
```

{% admonition(type="info", title="Note") %}
if you want to delete the symlinks for a folder, just use the `stow -D folder_name` command in the `.dotfiles` directory. 
{% end %}

<br>

Related links:
- [Dotfiles - Arch Wiki](https://wiki.archlinux.org/title/Dotfiles)
- [Stow - Gnu](https://www.gnu.org/software/stow/)
- [My Dotfiles](https://github.com/amiraref/dotfiles)
