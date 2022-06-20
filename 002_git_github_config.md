# Git & GitHub configuration

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

## SSH Keys

Don't bother with password and https. Use SSH keys to communicate with GitHub. Protect your keys with a strong passphrase. Use `ssh-agent` so that you don't have to re-type the passphrase everytype you interact with GitHub. You'd only have to do it once every morning. This practice strikes a good balance between no security (no passphrase) and convenience (typing passphrase all the time). This way if somebody steals your computer, there is less chance they can accept your GitHub account.

Use the [following guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

## PGP Key and commit verification

If you don't verify your commit with a PGP key, anyone puttin your name and email address in its `.gitconfig` can impersonate you. GitHub will assign the commits to you. 

If you configure PGP key, they will add the `verify` green flag to all your commits, meaning they were able to cryptographically verify that the owner of your private PGP key has signed it - and nobody else.

This is very useful in case of a GitHub security breach, like this one: 
- [GitHub App were able to upgrade to `scope:write` permissions if they were allowed `scope:read` permissions](https://news.ycombinator.com/item?id=31769520). That means that some GitHub App may have been able to push unverified commits associated impersonating any repository's member. Systematically using PGP commit verification and enforcing this as a rule on GitHub allows for mitigating such risks.

[Follow this guide](https://docs.github.com/en/authentication/managing-commit-signature-verification) to configure your PGP keys on GitHub.

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

The `git lg` alias is pretty handy to have a much cleaner version of `git log`.

The `url` part is very useful if you inadvertendly `git clone https://github.com/jvaljean/some/repository` instead of `git clone git@github.com:jvaljean:some/repository`. It will automatically convert the `https` url into a git-compliant `ssh` url.

## `.bashrc` or equivalent shell configuration

Setup your default editor:
```
export VISUAL="/opt/neovim/nvim.appimage"
export EDITOR="/opt/neovim/nvim.appimage"
```
This way it will open your favorite editor when you type `git commit` in your terminal for example.