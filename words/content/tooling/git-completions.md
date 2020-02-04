---
title: "G(it) Completions"
date: 2020-02-03T20:58:41-06:00
draft: false
---

I'm a big fan of spending 5 minutes on something if it will cut off a second of your time continuously. One of my daily use micro-optimizations involves typing `g` instead of `git`.

My goal:

1) Alias `git` to `g`
2) Add a few git command aliases that I frequently use
3) Add git completions for my new aliases

Note that I'm typically working in git bash on Windows in Cmder, so you may need to adjust these instructions slightly for your shell of choice.

## Alias `git` to `g`

Go to your home directory.

```
cd ~
```

Open up `.bashrc` (or whatever file your startup scripts run from.)

```
vi .bashrc
```

Add in the git alias, and while we're in here let's add one for gitk to open in a new process since I spam that throughout the day.

```
alias g='git'
alias gk='gitk &'
```

## Add git command aliases

Go to your home directory.

```
cd ~
```

Open up `.gitconfig` in your editor of choice.

```
vi .gitconfig
```

Add an `[alias]` section if it doesn't already exist with these following shortcuts and any more that you might want.

```
[user]
  email = yourEmail@example.org
  name = Your Name
[alias]
  co = checkout
  ci = commit
  s = status
  b = branch
```

Save that, reset your shell and try it out. Navigate to a git repo you know has a few branches.

```
git branch <tab>
```

You should see a few branches.

```
git b <tab>
```
Same result? Nice! Our alias is working.

```
g b <tab>
```
Same?! No?... Sad. Lets add our completions.


## Add git completions

Open your shell startup script file again.

```
vi ~/.bashrc
```

Append to the bottom

```
__git_complete g __git_main
```

That should alias `g` to your main `git` command and your `g b <tab>` from earlier should be working!

Reset your terminal and test.

That's it! Enjoy your newly found precious seconds every day.

([StackOverflow Source](https://stackoverflow.com/a/15009611/856692))

### Appendix: Wiring up git completions

Still here? Not working? In some combinations of ConsoleZ/Cmder/Git for Windows over the years I find that the default git completion isn't actually wired up. It's probably my fault due to some odd configuration, but I find that it's easiest to just grab a fresh copy of the completions rather than debug why they aren't already sourced. So if you find yourself in that situation, blaze forward with me. 

Navigate to a directory to save the completions.

```
cd /usr/share/bash-completion/completions
```

Download [git-completion.bash](https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash) to this directory

```
curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o git-completion.bash
```

If you get a permission issue, you can either run this as admin so it can save to your `Program Files` folder, or just save the file directly from github and move it in there. Your move. Just make sure you remove the defaulted `.txt` extension if you don't go the terminal path.

Ok! Now that we have the completions, we just need to wire it up.

Open that `.bashrc` file again from the previous step and modify your last line to be these two.

```
source /usr/share/bas-completion/completions/git-completion.bash
__git_complete g __git_main
```

Refresh your shell and you should be good to git gittin.

