---
title: "Pyenv is all you need"
date: 2021-02-02T15:34:30-04:00
categories:
  - blog
tags:
  - python
  - tech
---

We have all been there. You get your shiny new Mac or access to a new server and need to set up your python environment. Mostly you just want it all to work so you never have to touch it, but this time you decide to get fancy with it and start using virtual environments for stuff. That, or you need to because you need to use some open source stuff that happens to need a python version that is newer than the one you foolishly installed with brew. This time, you tell yourself, you will make nice little project specific environments and everything is going to work great. If I screw it up, i'll blast it away. There are plenty of good options for installing python nowadays, and many ways things can go wrong. 

In this post I'm going to cover a few of the key reasons why setting up your python environment is a pain, then give my go-to solution. 

## Pathing hell. 

Sometimes for whatever reason, your path gets screwed up and you try to run python but it just doesn't do what you want it to. Am I using system python or conda? brew? If I uninstall it, will something break?

## Command line nonsense.

If you're searching on stack overflow, then you've made wrong turn. I'm looking at you, brew. You shouldn't have to be making symlinks to libraries for things to work.

## Updating breaks everything

I have definitely fallen into this trap where I needed to update something, and then it broke my entire python install. 

# Solution 

Just use a higher level python manager. It lets you install whichever version of python you want and then specify whichever python you want to be active globally or locally, including system python. You can also blast it away really easily since it installs everything to your user directory. So you don't need sudo for anything.

**BONUS:** You can activate conda as the python environment/version of choice if you need some pesky package that happens to install better for your system with conda. It doesn't happen often, but I like having the option of activating anaconda as the base python for my virtual environment.



```bash
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash  

exec $SHELL
```

At this points, it will spit out some lines to add to your .bashrc/.zshrc/.bash_profile to find it on your path. The great thing is that it's a super clean remove if you hate it. Once you do that and restart or source terminal, then you can install a python, set it as global, or start making virtualenvs.


```bash
# install particular version and make it the default python with 'python' comman
pyenv install 3.8.0
pyenv global 3.8.0

# make a virtualenv
pyenv virtualenv 3.8.0 myCoolEnv
pyenv activate myCoolEnv

# Turn it off
pyenv deactivate
```

And honestly, that's about all there is to it. I let my python be python without worrying about tying it to brew, conda, or other stuff that install system libraries. And it just works.

Check out the [Pyenv Github][pyenv-github] for official documentation.

[pyenv-github]: https://github.com/pyenv/pyenv

