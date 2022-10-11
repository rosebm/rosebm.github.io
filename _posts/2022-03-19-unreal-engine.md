---
layout: post
title: 'My encounter with Unreal Engine'
tags: [Unreal Engine]
featured_image_thumbnail: /assets/images/posts/2022/unreal_engine/unreal_engine-wallpaper_thumbnail.jpg
featured_image: /assets/images/posts/2022/unreal_engine/unreal_engine-wallpaper.jpg
---

## My first steps with UE 

<!--more-->


Once, at one of my jobs we had a brief period where we were allowed to experiment with any side project we’d like, so I decided to go with the AR (Augmented Reality) and see what could bring for the company, that’s how I ended up creating some demos with Unity and Vuforia. Why Unity and not ARCore? Well, basically I felt that ARCore with Android Studio didn't have enough documentation, nor the community is large enough in comparison to Unity, which is vast. At the end, I enjoyed the experience with AR and wanted to continue to explore it.


Few years later, a friend of mine told me about this platform named Unreal Engine; although he uses it for architectural purposes, this engine created by Epic Games is also used for AR, game creation, animation, 3D content, movies… I mean, it was used to create the landscapes for The Mandalorian (enough to get my attention? Hell yeah!).

![Water in a shallow mountain valley in Kluane National Park](assets/images/posts/2022/unreal_engine/UE-mandalorian.jpeg)
<small>Images credit [Unreal Engine](https://www.unrealengine.com/en-US/blog/forging-new-paths-for-filmmakers-on-the-mandalorian)</small>

I then decided to follow some tutorials. As a beginner, I looked for those that teaches you the basic functionality and helps you to, step by step, create a small game (as you might know, the best way to learn is with your hands on!).

Coming from a world where I code everything (even the UI), I felt weird coding visually: dragging the nodes, the wires for connection, setting properties, etc (my brain thinks of them as classes, functions…); and that’s one advantage, you don’t need to know a programming language (in this case C++ for Unreal) to be able to release an app. Here some of basic concepts I have grasped:

### 1. Actor 
Is any object that can be placed into the world (or level).

### 2. Blueprints
Is an asset that contains data and instructions, allowing you to add behaviors to the object; since it’s based on a node system, you just need to create nodes and connect them (that’s your “coding” time). In a game is used to track the score, health, energy of a character, etc.

### 3. Node
Can have inputs and outputs. So it’s like a function that receives parameters, processes the data and returns the result

### 4. Mesh 
Is a collection of vertices, edges, and faces that describe the shape of a 3D object, for example, an apple. There’s 2 types:
- **Static mesh:** doesn't bend, deform or change shape in any way. Can move on the screen, but can't be animated
- **Skeletal mesh:** which is for objects with moving parts, such as a character


### 5. Material 
Determines how the surface of something looks, is like a layer you wrap the object with; in the case of the apple, will define its color or the texture (a 2D image).

## Dealing with Navigation Controls

Something funny I struggled with? Handling the editor controls on my Mac! Even though I have a mouse, I’m more comfortable using just the trackpad

- **Move forward/backward**

Single finger press on the trackpad and drag

![Rotate](assets/images/posts/2022/unreal_engine/ue-forward-backward-480.gif)

- **Move left/right**

Single finger press on the trackpad and drag to the left or right


![Rotate](assets/images/posts/2022/unreal_engine/ue-rotate-480.gif)


- **Move Up/Down**

Single finger press on the trackpad + key press (Q for down, E for up)

![Rotate](assets/images/posts/2022/unreal_engine/ue-up-down-480.gif)

You can find more controls here: [UE Viewport Controls](https://docs.unrealengine.com/5.0/en-US/viewport-controls-in-unreal-engine/)


## Time to install the APK 

Deploying the project on my device was straightforward (connect the device to the laptop and hit Play), now I wanted to build the app; went to the Package Project and selected the option for Android... nice! Just take the .apk and install it as usual, right? Well, no, here is the difference with a project built on Android Studio: if you drag the file into your device and try to execute it, you will receive errors such as: **"No Google Play Store Key. No OBB found and no store key to try to download. Please set one up in Android Project Settings"** 

Why this occurs? Because the option "Package game data inside .apk?" was unchecked, the project built into different files (.apk, .command, .obb), all necessary for the project to run. You can check the option, and the apk will be generated with all those files contained within. 

I must admit being shocked when I noticed that my basic project's size was around 700MB!, on Android you are always fighting to keep your app as light as possible.

So what's the way to install the apk? Through terminal, by running the .command file while the device is connected to the laptop. 

{% highlight kotlin %}
 $ Install_[ProjectName]_Development_armv7-es3.command
{% endhighlight %}

That's it! Was overwhelming getting all the information needed to start and understand UE; nevertheless, it has been fun, something I'll definitely continue digging into. 






