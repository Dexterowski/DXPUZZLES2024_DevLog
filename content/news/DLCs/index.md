---
title: DLCs, Main Menu, Importing my Map
date: 2024-05-27T15:34:47+08:00
featured: false
draft: true
comment: false
toc: false
reward: false
pinned: false
carousel: false
series:

categories:

tags: 
  - Unreal Engine 5.4
  - DLC
  - Addons
  - Modding
  - Mod loader
  - Main Menu
  - HammUEr
  - Source Engine
  - Steam
  - Steam Sessions
authors:
  - Dexterowski
images: ["images/maplist.png"]
---

This post will be a little messy because recently I did a misc things.

# Main Menu

I've imported Main Menu from my previous project for prototype it is good. This is my only one thing which **is not coded in C++* at the moment. As you can see I was inspired a lot of from Half Life main menu :D There is nothing at the background but this is not important for now.

Important thing is **Steam Sessions** to get it working, hosting a game and joining. I have this menu done working so I do not must code it for now. In addition I added to my **Host Game** window ability to get all available maps in the game and choose which one we want to play.

I have also done **Options** but it requires a lot of polishing, because player is able only to change graphics from profiles from lowest to highest.

IMAGE HERE

 In the lowest settings shadows and global illumination looks VERY VERY BAD. I must force that on lowest settings it will be set at least at medium quality level. I do not want to players look at my maps in almost **full-bright mode**

 IMAGE HERE - LOWEST SETTINGS

 So some advanced configuration would be nice but this is not necessary.

 # DLCs, modding, addons etc...

 If you read the [history and idea of DX PUZZLES](../what-is-dxpuzzles/index.md) you know that one of the core feature ones are ability to **make and play** custom maps made by community. So I must have in my game feature similar to DLCs. DLC is a simply an additional content to the game. The same would be in my case. I want community to do the custom maps and have ability to play it.

 > I do not know how to solve the problem of giving community an editor to make custom maps but this is 100% possible. Some games have such a possibility. 
 > * [UE-FN](https://dev.epicgames.com/community/fortnite/getting-started/uefn) - I know this is the product from Epic Games...
 > * [MechWarrior 5 MOD Kit](https://store.epicgames.com/en-US/p/mechwarrior-5--mod-editor) - This is the game not made by Epic Games and it has still custom editor to make mods for this game.
 > * [ARK MOD Kit](https://store.epicgames.com/pl/p/ark--modkit) - This is also pre-compiled editor for community to make mods


<center>

![](https://devotedstudios.com/wp-content/uploads/2023/05/EpdxkcWXMAIKtvL.webp "Unreal Engine: Fortnite Editor")

</center>


 I spent whole day to read about making modding support to UE5 game. It should meet some basic requirements like:
 * Custom maps should working independent of base game version (like in Half Life)
 * Easy install and use - something like: *Copy files to this folder and you're done!*
 * Additional content should be downloaded from server if client doesn't have it on local machine
