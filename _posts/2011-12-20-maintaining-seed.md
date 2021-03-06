---
layout: post
title: Maintaining the Seed Framework
tags: seed portability
category: blog
---

As you should know Seed Framework is a 2D cross platform game framework, it was used at TechFront Studios to develop casual games for Windows, MacOSX, iOS and Nintendo Wii.
That is not (too much of) a problem, as soon as the game is finished, for each build we created the project for the game and then compiled a release build. The most difficult part is to create a project for each IDE (in our case it was Visual Studio 2008, XCode and Metrowerks CodeWarrior), after that, everything went perfectly. 

I use linux at home as desktop for about 14 years, I normally use makefiles and vim for programming. At my work, I need to use windows with visual studio and, more recently, I�m using qtcreator. On the mac side, we have xcode.

The process of setting up the project is a hell, it could take from 30 minutes to about 2 days depending of the situation. Well, as we have make our framework open source, I thought it was, at least, needed to provide some project samples that anyone could use to start learning (I also have made an installer for windows, mac and linux but not published yet). Well, this is where the drama begins.

First, the Seed has some external dependencies like sdl, openal, ogg, vorbis and theora, to name a few. I had to manage a way to make the use of these dependencies transparent to the user, so I created a package called seed-dependency, where for windows and mac, we just extract it in the SDKROOT and then the projects will search the libraries there. It is composed of a version of each library for each compiler supported (vs, qtcreator and xcode). This was relatively easy. Also, I made a test to add most of the tiny dependencies direct in the project and not to use as a static library, well I got it with some work for QtCreator-Windows, it works but is very annoying to see how much fucking warnings these libraries have, they are old and the compilers today are more restrictive and scream for everything. The most fucked library is vorbis with tons and tons of warnings, with the �suggest parentheses around arithmetic in operand XYZ� being the big winner. I had make a bold move and started to reduce these warnings, I hope to not have changed the order of the calculations somewhere :). A side note: it is annoying how these old codes have line breaks at the column 80, there is no reason to keep with these retarded �standards� - but they are old, very old. I use vim for programming with a screen of 1280x1024 and I have a lot of columns to use, why do not use it?

Second, the task to create the projects for each IDE and for each platform for each subproject (library, tools and samples). This is huge. It takes so much time that I already give up of doing this about ten times. It is so annoying and so repetitive that it make me get tired so fast, but I need to do it (if I want an installer for each platform that just works). Currently, I had created the projects for Visual Studio 2008 (2010 not yet!), QtCreator for Windows, XCode and pure Makefiles for Linux. I still need to fix the QtCreator projects to work on Linux also. But don�t think that once this is done it is done, keeping it up-to-date is as hard or harder than setting up everything. I lost more time in this than coding, and this is obviously not good.

Then you can ask me, why not use CMake? Well, it almost as hard as doing each one separated and the generated Visual Studio project looks like an spaghetti. When I tried it, it haven�t support for QtCreator and it was hard to get XCode to work correctly. Anyway, CMake may be the solution today (if I knew how to use it better), maybe someday I will look at it again. My Cmake project is on GitHub also, I just doesn�t use it.

The thing is, making the C or C++ code cross platform is really really easy, making projects for users be able to use it in a cross platform way is the hardest part.
