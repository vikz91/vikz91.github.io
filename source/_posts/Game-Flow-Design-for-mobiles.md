---
title: Game Flow Design for mobiles
date: 2015-02-17 00:27:51
tags:
- Architecture
- Game Flow Diagram
- Mobile Workflow
category: Game Development
---


# Game Flow Design for mobiles

Recently while making one of my games,  my team felt the need to have a standardized game Flow Diagram so that every one in the team clearly understands how the game progression works, and which screens comes after which one. 

<!-- more -->

## Game flow? for mobiles?

So, I [stumbled upon this](http://tinypic.com/view.php?pic=2087ac3&s=5#.WHaAvLZ95Z2), and modified it to my own needs and here it is. For mobile devices, Social Leaderboard and IAP are greatly used.

![](/2015/02/17/Game-Flow-Design-for-mobiles/What-a-Drag-Diagrams.jpg)


## How does it work?

1. User taps/clicks the game icon from the app drawer.
2. Game Launches with a splash screen, showing the company logo and other frameworks / Libraries used ( if permissible). The Intro Splash screen must not retain more than 3 secs, and should fade to either white, or black.
3. If an initial Loading screen is intended (which is recommended), The graphics used should be minimal and should to add to the overhead. It can also display Tips and/or News.
4. The Main Menu should be very attractive to give its FIRST IMPRESSION. Do NOT over do the Main Menu, also, try to avoid bluntness. It is the centralized Menu, which will take the user to all the other screens,a nd even QUIT the game too.
5. Social Leaderboard, IAP and Missions are the “minimum and must have” features of a mobile game nowdays. If you are dreaming of earning through Ads alone, then good luck keep on dreaming!
6. For mobile device, Settings Popup should atleast – sound toggles, Reset Score, Vibration Toggle, etc.
7. About Popup must contain a brief intro about the game, and should clearly mention the goal of the game and the alternative links. Also, this is a very good place for embedding Credits.
8. Technically, you can give as many Game Modes as you wish. But do take note that you should also provide an Interactive Tutorial for each.
9. A Briefing Page us what tells the players about the goals and missions of the level he is going to play. For e.g. Score Bonuses and Unlocks, Friendly Challenges, etc.
10. Action is where the actual game takes place. One tip of advice – NEVER, EVER, EVER put advertisements on the main game scene. Gamers will hate it and will be frustrated.
11. Game Flow Design lets us investigate the flow of all the screens .
12. Both Action and the interactive tutorial should have Pause Buttons.
13. A Pause Popup should allow the game to navigate to Main Menu or, resume back to the ongoing game.
14. Debriefing Popup should inform the user if he/she won or lose. Score calculation happens at these stage and all the bonus unlocks should be displayed here. On of the critical responsibilities of a game designers is to let the gamer feel paradise after winning a game. Rewards, and exclusive REWARDS. Remember, Most players play games for appreciations, even virtual ones.