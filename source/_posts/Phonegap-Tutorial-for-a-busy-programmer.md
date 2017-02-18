---
title: Phonegap Tutorial for a busy programmer
date: 2017-02-02 01:28:48
tags: 
- Phonegap
- js
- hybrid app
- mobile app development
category: Mobile App
---

## As the title says, lets get to it right away

As [they](http://phonegap.com/about/) say, "Building applications for each platform–iPhone, Android, Windows and more–requires different frameworks and languages. PhoneGap solves this by using standards-based web technologies to bridge web applications and mobile devices. "

<!-- more -->
## Note
This tutorial is for Android till now. iOS codes will be updated as soon as you gift me an iPhone :P

## Prerequisites
1. NodeJS >= 4.x
2. npm >= 3.x
3. A good text editor (hint: Sublime)
4. Chrome (works best)
5. Basic Terminal Commands (cd, ls, etc.)
6. Android SDK
7. JDK >= 7
8. Ant (optional)


## Setting up Environment

1. Install [NodeJS](https://nodejs.org/en/)
2. Update NPM ( node Package Manager) 
	a. Mac : ``npm install -g npm``
	b: Others: `` npm update -g npm``
3. Install Sublime Text Editor or [Atom](https://atom.io/)
4. Install [Android SDK](https://developer.android.com/studio/index.html) ``You don't need Android Studio but it eases a lot of stuff``
5. Install [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html)
6. Make Sure JAVA environment variables and ANT is properly set in [windows](https://www.mkyong.com/ant/how-to-install-apache-ant-on-windows/) and [Mac](http://www.sajeconsultants.com/how-to-set-java_home-on-mac-os-x/)
7. Reboot PC (optional)
8. npm install -g phonegap // install phonegap globally 

## First 'Hello World'

1. In Terminal, create a new directory for your projects `` mkdir phonegapApps && cd phonegapApps``
2. Scaffold phonegap app : ``phonegap create helloworld``. This will download dependencies as well.
3. Open index.html and change something (e.g. your name in a new div). Save and close.
4. Try viewing it : ``phonegap serve``. Open the link in your mobile or browser.
5. Keep changing stuff and refresh the page, till you are happy.
6. Change 'config.xml' with your name and other details
7. Add Android Platform to your project ``cordova platform add android --save`` (phonegap is just Adobe's version but basically its just the SAME)
8. Build the damn thing ``phonegap build android``
9. Transfer the debug apk from ``.\platforms\android\build\outputs\apk\`` to your phone via [Airdroid](https://play.google.com/store/apps/details?id=com.sand.airdroid&hl=en) or USB
10. Change your security settings in phone to allow installing apps outside Google Play. 
11. Enjoy!


## Signing for Production
1. Make sure your bundle IDentifier is properly set 'com.company.productname'
2. Make Sure proper icons are settled
3. Sign in via keytool (java) : ``keytool -genkey -v -keystore my_keystore.keystore 
   -alias TutorialsPoint -keyalg RSA -keysize 2048 -validity 10000``
4. More guidelines will be coming up.


## But why though?
A firend of mine (yes, its you bittu) needed a quick cheat sheet for phonegap as he is very busy with his work. So hence, this is it.
![Deal with it](https://media.giphy.com/media/CEF7ocyS0C9q/200_s.gif)


## Bump
A full tutorial on making a chat app in phonegap will be uploaded in near future that will use Socket.IO, Phone Number Verification and end-to-end encryption using [signal server](http://debabhishek.com/writes/Installing-and-Running-TextSecure-Signal-Server-on-Windows/).
