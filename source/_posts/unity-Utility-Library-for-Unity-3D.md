---
title: '_unity : Utility Library for Unity 3D'
date: 2014-8-15 00:11:16
tags: 
- debug
- Github
- gui scaling
- library
- shuffle
- utility
- finite state machine
- generic singleton
- REST client
- rapid tools
- rapid library
Category: Game Development
---

# _unity : Utility Library for Unity 3D

This is my take on a utility library for Unity Engine. It has some GUI Scaling Utility Methods, some Debugging Methods, Array Utilities and some other cool stuff like finite state machine, generic singletons, Event Messaging, and etc. 

This is an ongoing project which I’ll be updating over time.

[_Unity Utility Library for Unity Game Engine](https://bitbucket.org/skipbits/_unity)


## Usage
1. Download from Github
2. Compile with VS 2013
3. Put dll in Plugins folder of Unity Project

## Quick Setup
Get the pre-compiled binary dll in bin directory of this repo and put it in plugins folder of your unity project.


## DOCS

public static void l(string msg)
prints a string to the debug console with current time and date and other info e.g. _.l(“Hello”);

public static void ExecOnce(Callback callback,ref bool limiter)
Executes a method only once inside all loop functions ( update, OnGUI, etc)

public static void GUISetup(float customWidth=1366f,float customHeight=768f)
Sets up GUI Scaling correctly. Call this at first line of each OnGUI. e.g. _.GUISetup(); e.g. _.GUISetup(800f,480f);

Find more at the Bitbucket repo.

Feel free to ask any questions regarding _unity as this is a very basic utility library for Unity 3D engine. Catch me on google @ +AbhishekDeb.


