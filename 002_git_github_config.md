# Git & GitHub configuration

Some useful tips for git integration and configuration.
For git workflow, including rebase and such - refer to [my opinionated git workflow](https://github.com/nicobao/personal-notebook/blob/main/003_opinionated_git_workflow.md).

## Git hooks

By default git will allow you to commit conflicts that have not been fixed!
It's a very common scenario, especially when going through long merge/rebase.

What if we could prevent that from happening? This is solved by using a `pre-commit` git hook.

We could do that by hand - [like this](https://gist.github.com/maxmarkus/8a81edd58dd65a45731f).
We can use [`git-templates`](https://coderwall.com/p/jp7d5q/create-a-global-git-commit-hook) to add those hooks globally.

But there is a better way - with already pre-defined handy pre-commit hooks.
It is also actionnable by the CI:

https://pre-commit.com/


## SSH Keys

Don't bother with password and https. Use SSH keys to communicate with GitHub. Protect your keys with a strong passphrase. Use `ssh-agent` so that you don't have to re-type the passphrase everytype you interact with GitHub. You'd only have to do it once every morning. This practice strikes a good balance between no security (no passphrase) and convenience (typing passphrase all the time). This way if somebody steals your computer, there is less chance they can accept your GitHub account.

Use the [following guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

## GPG Key and commit verification

If you don't verify your commit with a GPG key, anyone putting your name and email address in its `.gitconfig` can impersonate you. GitHub will assign the commits to you.

If you configure GPG key, they will add the `verify` green flag to all your commits, meaning they were able to cryptographically verify that the owner of your private GPG key has signed it - and nobody else.

This is very useful in case of a GitHub security breach, like this one: 
- [GitHub App were able to upgrade to `scope:write` permissions if they were allowed `scope:read` permissions](https://news.ycombinator.com/item?id=31769520). That means that some GitHub App may have been able to push unverified commits associated impersonating any repository's member. Systematically using GPG commit verification and enforcing this as a rule on GitHub allows for mitigating such risks.

[Follow this guide](https://docs.github.com/en/authentication/managing-commit-signature-verification) to configure your GPG keys on GitHub.

## `~/.gitconfig`

Replace the mergetool `cmd` with your tool of choice (`code` or `idea` or whatever IDE/Text editor you use).
Doing so, it will open your favorite editor when you happen to have to resolve git conflicts after throwing a git command to your terminal.

[Check out this .gitconfig](https://github.com/nicobao/setup/blob/master/.gitconfig)

The `git lg` alias is pretty handy to have a much cleaner version of `git log`.

The `url` part is very useful if you inadvertendly `git clone https://github.com/jvaljean/some/repository` instead of `git clone git@github.com:jvaljean:some/repository`. It will automatically convert the `https` url into a git-compliant `ssh` url.

I don't see any valid reason when you'd want to use `git pull` in merge mode rather than in rebase mode (see [my workflow](003_opinionated_git_workflow.md)).

## `.bashrc` or equivalent shell configuration

Setup your default editor:
```
export VISUAL="/opt/neovim/nvim.appimage"
export EDITOR="/opt/neovim/nvim.appimage"
```
This way it will open your favorite editor when you type `git commit` in your terminal for example.
