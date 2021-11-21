---
title: "iPad Workflow"
layout: post
post-image: images/assets/man-g943b404e3_640.jpg
description: Get the most out of just an iPad.
tags:
   - paperless
   - tech
   - iPad
---


I have a 14 year old macbook pro that barely works. I ended up getting an iPad pro to supplement my technical work (see “going paperless”), but it’s really not designed for technical work. There are some good native tools for basic python and web development if that is your thing, but support is limited for doing things like jumping on the command line and compiling bleeding edge libraries. Typically if you want to do something like that, you need to ssh into an external server or google colab or some other cloud server. If you don’t mind doing that kind of thing, then you can actually get pretty far. I’m a big fan of overleaf, for example, for collaborating on latex documents. That kind of cloud-based writing is amazing on a tablet.

The problem is, who wants to use a raspberry pi or other server workarounds just to use some standard tools that really should be native on a piece of hardware with that much juice? My blog uses as markdown and github. I don’t want to buy a nice gui git application just so I can “git push” a text file. Call me old fashioned, but I still prefer the command line when I git. But I like doing my creative writing on the tablet. It’s lightweight with a minimal interface that’s conducive to taking with me and letting the ideas flow.

## Make Peace with the Lack of Features
Over a year of seeing what works well and what doesn’t, I had to do some real soul searching. I am okay with not having the tablet be my workhorse. After trying the raspberry pi, google colab and code-server workarounds, I have decided that it takes enough away from the experience that I don’t enjoy doing the actual building and developing on it. I end up spending too much effort fighting the tools. If you want a jack of all trades, get a surface or 2-in-1 type device. Windows is actually pretty legit now with WSL. 

## Lean into the Reading, Writing, and Creating - Limit the Extra
After a year of tinkering, I have settled in on a few great tools. I have also discovered some quite recently that inspired this post on my workflow. My advice for someone figuring out how to make an iPad work for them is to use it for the writing, reading, planning, and designing. Save the coding and building for elsewhere unless you find tools that you really like for your projects. I have listed a few of the other tools that can be useful in a pinch.

### Blogs and Short Documents
Bear supports markdown. It’s what i’m using right now. I don’t subscribe or anything so I don’t have sync support. I keep documents short lived in Bear. It’s meant to be somewhat of a staging area for stuff that I want to write quickly and move to a longer-lasting venue like my blog or another app.

### Novels or Longer Literature
**Scrivener** is excellent for longer prose like novels or memoirs that is easier to write in chunks and stitch together. My brain fires ideas for multiple sections all the time when i’m writing. Scrivener let’s you write them all in any order you want and in separate sections  (files) that can be reordered as you see fit and organized into chapters or other groupings however you want. I tried using the built-in notes app for a while, but it makes reordering too cumbersome. Finally, the file sync with dropbox works very well.

### Remote File Edits
I found **Koder** to work pretty well for jumping into my pi using sftp for some quick code editing. I still don’t love it, but it works. Using ssh command line itself doesn’t work very well. The colors and syntax get messed up. It has some syntax highlighting. I don’t love it, but for firing up remote files it works pretty well.

### SSH
**Termius** works great for a quick remote connect over ssh in a free package. I moved away from doing this kind of thing, but it does the job.

### PDF
**PDF Viewer** is by far the best app for reading and annotating PDFs. It saves right away and sync with iCloud or Dropbox works quite well if you have a premium Dropbox account. It has the color inversion for eye fatigue, and overall is the best of the bunch I have tried.

### Scanning Documents
I use the built in **Notes** app for this. Hit the little camera button at the top and touch scan. It works very well.

### Encrypting PDFs
After scanning, you might want to email something with sensitive data. If you’re on a legit Mac, then exporting from ** Preview** gives you the encryption option. However, the built in tools don’t give you the encryption option and most of the pdf apps that encrypt are either janky, riddles with ads, or require a subscription. I finally found one I like that is simple, free, and does the trick: **QuickSecure**.

### Actual git, finally (and bonus, vim)!
So here it is, I found an app called **ish** that emulates linux on the ipad. Use that, then install git, vim, and whatever else you like to use that is supported. It even supports Vundle for vim which was a nice surprise and the colors work for themes and syntax highlighting. I wouldn’t try to use it as some kind of virtual machine for actual computing, but it lets me push changes to my blog using the command line, which makes me happy :D. You can even install python. Go crazy. It’s the simple things.

#blog
