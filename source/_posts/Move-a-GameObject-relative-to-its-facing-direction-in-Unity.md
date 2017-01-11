---
title: Move a GameObject relative to its facing direction in Unity
tags: 
- movement
- transform
- unity3d
- unity
- physics
- rigidbody
- C#
category: Game Development
date: 2014-06-01 
---


# The Scenario

Generally in a game where the player has to move a gameobject (Central character, RPG, Hero, etc.) as he progresses. Now, assuming the game to be non-unidirectional, many people (including me) face the problem of making the gameobject move relative to its facing direction.


If the game is such that the player can simultaneously move on x and z direction, you can independently setup the movement physics code. Turning around would be used by the mouse itself so you just need to setup a mouse-look script on the camera. Here the Turning around is a continuous task (e.g. Crysis, COD, Project IGI).

e.g.
```
//Moving Forward
 rigidbody.AddForce(Vector3.forward* Input.GetAxis(“Vertical”)*400);

//Turning Around
rigidbody.AddForce(Vector3.right* Input.GetAxis(“Horizontal”)*400);
```
But there may be a problem if you want to make a game where the mouse-look isn’t really needed and the turning happens discretely by user input and based on the direction the player is facing, he is needed to move forward (e.g. Temple Run).

 

# The Solution

Now you can either use global variables to keep reference of the direction the player gameObject is facing, or use simple and more natural logic to move it, using the following scripts.

## Case I

If you are manipulating the position directly, you can move a gameobject relative to its facing direction by using transform.forward instead of Vector3.forward in the scripting.

e.g.
```
transform.position=Vector3.Lerp(transform.position,transform.forward,Time.deltaTime);
```

### Case II

If your code is using physics, use rigidbody.AddRelativeForce rather than rigidbody.AddForce

e.g.

```
rigidbody.AddRelativeForce(Vector3.forward*30);
``` 

### Note

The scripts are intended for 3D Environment. But changing them for 2D is easy. I will try to provide 2D examples too if I find suitable time.