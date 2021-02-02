---
title: "Pyenv is all you need"
date: 2021-02-02T15:34:30-04:00
categories:
  - blog
tags:
  - python
  - tech
---

We have all been there. You get your shiny new Mac or access to a new server and need to set up your python environment. Mostly you just want it all to work so you never have to touch it, but this time you decide to get fancy with it and start using virtual environments for stuff. That, or you need to because you need to use some open source stuff that happens to need a python version that is newer than the one you foolishly installed with brew. This time, you tell yourself, you will make nice little project specific environments and everything is going to work great. If I screw it up, i'll blast it away.  But then it doesn't work. Why? In this post I'm going to cover a few of the key reasons why setting up your python environment is a pain, and the saving gracxe

## Pathing hell. 

Something needs a different something. Which python am I using now? I tried to call pip3 to install something, but somehow it called a different pip than the one I wanted.

## Command line nonsense.

If you're searching on stack overflow, then you've made wrong turn. I'm looking at you, brew.

# Solution 

Just use a higher level python manager. It lets you install whichever version of python you want and then specify whichever python you want to be active globally or locally, including system python. You can also blast it away really easily since it installs everything to your user directory. So you don't need sudo for anything.

**BONUS:** You can activate conda as the python environment/version of choice if you need some pesky package that happens to install better for your system with conda. It doesn't happen often, but I like having the option of activating anaconda as the base python for my virtual environment.



```bash
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash  

exec $SHELL
```

Check out the [Pyenv Github][pyenv-github] for official documentation.

[pyenv-github]: https://github.com/pyenv/pyenv

