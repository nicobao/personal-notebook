# How to configure git locally?

Some useful tips for git integration and configuration.
For git workflow, including rebase and such - see another document (TODO).

## Git hooks

By default git will allow you to commit conflicts that have not been fixed!
It's a very common scenario, especially when going through long merge/rebase.

This is solved by using the pre-existing `pre-commit` git hook.
We just need to activate it.

1. If there is no existing git repository on your machine, create one using the following commands:
	- `mkdir temp-git`
	- `cd temp-git`
	- `git init`
1. Enable git templates
	- `git config --global init.templatedirs '~/.git-templates'`
1. Create a directory to hold the global hooks:
	- `mkdir -p ~/.git-templates/hooks`
1. Copy the `pre-commit.sample` hook to the global hooks directory:
	- `cp temp-git/.git/hooks/pre-commit.sample ~/.git-templates/hooks/pre-commit`
1. Make sure the hook is executable
	- `ls -lrt ~/.git-templates/hooks`
	- `chmod 755 ~/.git-templates/hooks/pre-commit`
1. Re-initialize git in each existing repo you'd like to use the global hooks in:
	- `cd temp-git`
	- `git init`

## `gitconfig`

Replace the mergetool `cmd` with your tool of choice (`code` or `idea` or whatever IDE/Text editor you use).
Doing so, it will open your favorite editor when you happen to have to resolve git conflicts after throwing a git command to your terminal.
```
[user]
	email = name@example.com
	name = Jean Valjean
[merge]
	conflictstyle = diff3
	tool = vimdiff

[mergetool "vimdiff"]
	cmd = /opt/neovim/nvim.appimage -d $LOCAL $BASE $REMOTE $MERGED -c '$wincmd w' -c 'wincmd J'

[alias]
	lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
[url "git@github.com:"]
	insteadOf = https://github.com/
[init]
	templatedir = ~/.git-templates
```

## `.bashrc` or equivalent shell configuration

Setup your default editor:
```
export VISUAL="/opt/neovim/nvim.appimage"
export EDITOR="/opt/neovim/nvim.appimage"
```
This way it will open your favorite editor when you type `git commit` in your terminal for example.
