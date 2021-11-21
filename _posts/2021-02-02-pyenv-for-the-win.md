---
title: "Pyenv is all you need"
date: 2021-02-02T15:34:30-04:00
layout: post
post-image: images/assets/desk-g9f3ed8c75_640.jpg
description: Stop with the shenanigans.
tags:
  - python
  - tech
---


We have all been there. You get your shiny new Mac or access to a new server and need to set up your python environment. Mostly you just want it all to work so you never have to touch it, but this time you decide to get fancy with it and start using virtual environments for stuff. That, or you need to because you need to use some open source stuff that happens to need a python version that is newer than the one you foolishly installed with brew. This time, you tell yourself, you will make nice little project specific environments and everything is going to work great. Fortunately, there are plenty of good options for installing python nowadays. Unfortunately, that means that there are many ways things can go wrong. I mainly develop in a Mac/Linux environment and have tried many python management systems over the years. Here's what I have faced during my dark brew and system python days.

## Pathing confusion

Sometimes for whatever reason, your path gets screwed up and you try to run python but it just doesn't do what you want it to. Am I using system python or conda? brew? If I uninstall it, will something break?

## Command line nonsense

If you're searching on stack overflow, then you've made wrong turn. I'm looking at you, brew. You shouldn't have to be making symlinks to libraries for things to work.

## Everything is broken

I have definitely fallen into this trap where I needed to update something, and then it broke my entire python install. 

# Solution 

Be kind to yourself, my friend! Now I avoid conda and brew at all costs and just stick to pyenv. It gives me the control and flexibility I need to install whatever I need with the freedom to blast it away as if it was never there (it shouldn't break other libraries on my system). 

Pyenv is a kind of meta-layer python manager, similar to conda, but with additional control to switch to using any python version or even system python. You can easily keep your system python (or your favorite one without remembering to type the 3) the default while having certain virtualenvs be automatically active in certain project directories. It all installs to your user directory so you can blast it and you can install any libraries without sudo. 

**BONUS:** You can activate conda as the python environment/version of choice if you need some pesky package that happens to install better for your system with conda. It doesn't happen often, but I like having the option of activating anaconda as the base python for my virtual environment. Conda does a great job of pulling in necessary cuda dependencies for pytorch libraries, for example, without requiring a system admin to set it cuda and other related libraries.

Also, it's so EASY!

```bash
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash  

exec $SHELL
```

At this point, it will spit out some lines to add to your .bashrc/.zshrc/.bash_profile to find it on your path. The great thing is that it's a super clean remove if you hate it. Once you do that and restart or source terminal, then you can install a python, set it as global, or start making virtualenvs.


```bash
# install particular version and make it the default python with 'python' command
pyenv install 3.8.0
pyenv global 3.8.0

# show me which I have installed
pyenv versions

# make the system the default python
pyenv global system

# make a virtualenv
pyenv virtualenv 3.8.0 myCoolEnv
pyenv activate myCoolEnv

# better yet, turn it off but have it be the default for the current directory 
pyenv deactivate
pyenv local myCoolEnv
```

And honestly, that's about all there is to it. I let my python be python without worrying about tying it to brew, conda, or other stuff that installs system libraries. And it just works. 

Check out the [Pyenv Github][pyenv-github] for official documentation. There are some cases where you may need to install some required libraries. It's documented pretty well over there. 


[pyenv-github]: https://github.com/pyenv/pyenv

