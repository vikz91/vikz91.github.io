---
title: Lifesaver tools and scripts
date: 2014-08-05 23:54:33
tags:
- plugins
- scripts 
- tools
- tricks
category: Game Development
---

# Lifesaver tools and scripts

Here are some cool and “must-have” tools, scripts, and Lifesaver plugins that will save your from pulling your hairs out. All of these are FREE.

1. Frame Per Second
	If you are using Unity’s FPS to track and manage the game, think again. Use [this script](http://wiki.unity3d.com/index.php?title=FramesPerSecond) to see REAL FPS counter.

2. Finite State Machine
	Every good programmer uses a state machine solution to avoid code complexity and re-use code in a structured way. So far, the [best Finite State Machine Solution](https://github.com/thefuntastic/Unity3d-Finite-State-Machine) I have got. You could be interested in [uTool](https://bitbucket.org/skipbits/_unity) plugin as well which has integrated State Machine Features

3. Directory Contents to Text File
	In Windows, if you want to print all the contents of a directory ( may be, recursively?) to an external text file, do so by the tree command. Hold Shift and Right Click the directory you want to print, click on Open Command Window here and paste this code and press enter.  
	` tree /F /A > contents.txt `

	The `/F` Flag prints the file names, and `/A` flag disables extended ASCII printing.  
	IMHO, this should have topped the Lifesaver tools and scripts list.

4. ~~In-App Purchase, Social Profile, Rewards~~ *This has been discontinued*. 
	Recently I stumbled upon a great open-source framework for unity: Soomla.  
	*Soomla offers :*  
	Store : The one-go IAP Package for all the virtual currency management
	Level Up : Game Progression with ease.
	Grow : Community driven analytic and data sharing
	Profile: Social Profile for your players – Facebook Status, Twitter Bragging, etc.
	The best thing about this is that the whole package is open-source and you can use them as long as you want, modify them as you wish.

5. Shaders  
	Shaders contain code that defines what kind of properties and assets to use. A Shader basically defines a formula for how the in-game shading should look. Shaders are meant to be written by graphics programmers. They are created using the ShaderLab language, which is quite simple. For a quick view, these are the most used ones:  
	- [Transparent Single Color Shader](http://wiki.unity3d.com/index.php/Transparent_Single_Color_Shader)
	- [Toon Shadowed Shader](http://wiki.unity3d.com/index.php/ToonShadowed)
	- [Outlined Diffuse Shader](http://wiki.unity3d.com/index.php/OutlinedDiffuse)
	- [Quick and Easy Toon Shader from Asset Store](https://www.assetstore.unity3d.com/#/content/3926)  

6. Input and Touches
	Prime31 does offer a good touch solution but if you are looking for more intuitive and all-in-one touch solutions, you have to look at [TouchScript](https://github.com/InteractiveLab/TouchScript) .
		+ Good Documentation
		+ Demo Tutorials  
		+ Comparatively Easy and Fast  
		+ Many Projects are using them   
7. Tweening
	To the point iTween is considered the defacto for tweening in Unity Systems , I found [DOTween](http://dotween.demigiant.com/).  
	Is really Fast ( and I mean REALLY), fuly optimized for C# and works great on every device.  
	Another one of the neat Lifesaver tools and scripts for unity to strap under your belt.