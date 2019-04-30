--- 
title: "Running Davinci Resolve on Arch" 
description: "My adventure with Davinci Resolve Beta 16 but on Arch Linux" 
author: "Skye" 
date: 2019-04-17T21:12:08-04:00 
draft: false
---
Let me just start this post with this: oh god

I wanted to run Davinci Resolve on my Arch Linux PC to do some video editing for a school project. Now you're probably saying "Just use OpenShot, Kdenlive, or even Blender" and while yes that can edit, they're not stable for me and are missing some features that I need/would like to have.
So I decided to use Davinci Resolve, problem is that I *never* got it to work before. I mean ***never***.
On Ubuntu, Fedora, or Arch Linux. Yet other people could run it fine on these systems. Still, I *really* wanted to run Davinci Resolve and figure out why it wouldn't work.

I started out by using the AUR package [davinci-resolve-beta](https://aur.archlinux.org/packages/davinci-resolve-beta/)
Yet it didn't work, it threw an error about OpenCL and how none of my card support it. This was confusing because my GPU (An R9 380) worked with Davinci on *Windows*  and when I googled this error, it was filled with people having issue with their Intel iGPU. (Yes my iGPU was disabled.)
I then tried checking if opencl-mesa was installed, it wasn't. So I then thought to myself "Ah ha, OpenCL isn't actually *installed*". I went ahead and installed it, *annnnd* it still crashed. Giving me the same error.

Now I'm stuck here, thinking to myself "Is my card blacklisted on Linux? Does it just not work at all?"

Then, I decided to check the logs more deeply. (Lesson of the day BTW)

I find this "| INFO  | 2019-04-29 17:09:01,894 | Skipping unqualified OpenCL platform: Clover"

Clover isn't the name of my generation's card name? Where could it be getting that name from?
Turns out Mesa's OpenCL is called Clover and Davinci Resolve doesn't work with the open source drivers.

I go install [opencl-amd](), a version of the OpenCL included with AMDGPU-PRO drivers but made to work with the open source AMDGPU drivers.

I try to open Davinci Resolve and IT FINDS MY CARD, but it doesn't open? Still stuck in the same place?
Turns out it's still trying to load the Mesa OpenCL so I go ahead and uninstall that.

I open it... AND PROGRESS HAS BEEN MADE. IT GETS PAST LOOKING FOR CONTROL SURFACES AND MORE STUFF APPEARS IN THE LOG.
IT STILL CRASHES BUT I SEE [THIS](), A PROBLEM REPORT FOR DAVINCI RESOLVE. A WINDOW HAS BEEN OPENED.

At this point it crashes around loading the fairlight ui and oh lord I made the mistake of clicking "Don't Show Me Again" 

Time to reinstall DaVinci Resolve and hope it resets my user settings.
OH BUT IT DOESN'T BECAUSE IN THE INSTALLER LOG I SEE "/var/BlackmagicDesign/DaVinci Resolve"
YAY, atleast I found what to delete. Of course, deleting that doesn't work.
At this point this is my 5th reinstall.
Can you guess what happened? That's right NOTHING HAPPENED!

Looking on the fourms, you appearently need an intel iGPU to help you run it. /shrug I have no idea what it means by this.
So I decided to enable my iGPU in my BIOS. 

TLDR:
Install opencl-amd and uninstall opencl-mesa if trying to use DaVinci Resolve on a machine with an AMD graphics card.
