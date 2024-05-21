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
Hi, in this article I want to explain and talk about the **dotfiles**.
The first question is what are the dotfiles ?
It's very clear, the files that their names starts with a dot `.` which (in most of operating systems) is a **hidden** file.

# Why the dotfiles are important ?
If you are a user of a Unix-like operating system (like GNU/LINUX distributions, macOS, etc), the dotfiles are too important for you.
Most of the applications use their specific dotfile to store their configuration.
For example have you ever used the **vim** editor? The vim editor loads it's configuration from the `.vimrc` which is located in your home directory (i.e. `~/.vimrc`).
If you explore on your home directory and the `~/.config/` directory, you will probably see many dotfiles from different software (see hidden files with command : `ls -a`) such as `.bashrc`, `.zshrc`, `.gitconfig`, etc.

## Benefits of Tracking the dotfiles
After some time that you have struggled with many of software configurations to make them customized for your likings (specially in Linux), you probably try to **keep** your dotfiles somewhere to use them in the future or to use on the other machines.
So after you save your specific dotfiles, you just need to copy them in your new machine and enjoy working with the same configuration and customization that you have had on the other machine.

# How Keep and track the dotfiles ?
One of the best ways to store the dotfiles is using by **Git**, but managing your dotfiles using git is a little different that tracking your project's source code.
In the past and until recently, I was using an old method to store my dotfiles with git that was not too optimized. In this old method you have to create a `bare` repository and use the alias.
I don't want to explain this method here and my reason for that is that in my experience I found this method is not too optimized with **shell ta autocompletion**, and most of the times I had problems with that, but if you interested in that, you can find it with the **_"bare repository and alias method"_** in links that I will put below.


## Track your dotfiles using the stow
There is a useful program named **[stow](https://www.gnu.org/software/stow/)** which is a symlink farm manager. The stow has many usages but it can provide a structured approach to store your dotfiles.

1. first, make sure the **stow** is installed on your machine, in most of Linux distros it's already installed, but if not, install it using your package manager
2. then you should create a directory to store your dotfiles in your **home directory**, in this case we name it `.dotfiles`
```bash
mkdir ~/.dotfiles
cd ~/.dotfiles
```
3. next, you should create your dotfiles in subdirectory of this directory (or move them to it).
Let's imagine we have `~/.vimrc` file in the home directory, you should move it to a new folder on `.dotfiles` directory :
```bash
mkdir vim
mv ~/.vimrc ~/.dotfiles/vim/
```

{% admonition(type="tip", title="Tip") %}
The name of subdirectory can be anything you want, but it's better to use a related name to make the files structure more clear.
{% end %}

You should consider that the **stow** will follow the same folder structure that is placed in home directory.
For example, the neovim's config folder that is typically placed in `~/.config/nvim/` would be like : `~/.dotfiles/nvim/.config/nvim/`.
Do the same thing for all of your dotfiles.

The following is the structure of my `.dotfiles` directory ([my dotfiles repository](https://github.com/AmirAref/dotfiles/)):
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

4. run the following command to make symlinks using stow (put your own folders' names) :
```bash
stow git vim nvim kitty
```
Now if you go to your home directory, you'll see the **symlinks** have been created :
```bash
➜ ls -al .vimrc
20 May  6 00:32 .vimrc -> .dotfiles/vim/.vimrc
```

5. finally, initialize git in your `.dotfiles` folder, and then create a commit and push it to your online git repository :
```bash
cd ~/.dotfiles/
git init
git commit -m "Initial Commit"
git push origin master
```

{% admonition(type="info", title="Note") %}
If you want to delete the symlinks for a folder, just use the `stow -D folder_name` command in the `.dotfiles` directory. 
{% end %}

<br>

Related links:
- [Dotfiles - Arch Wiki](https://wiki.archlinux.org/title/Dotfiles)
- [Stow - Gnu](https://www.gnu.org/software/stow/)
- [My Dotfiles](https://github.com/amiraref/dotfiles)
