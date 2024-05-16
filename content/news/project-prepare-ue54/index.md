---
# type: docs 
title: Preparing project for Unreal Engine 5.4
date: 2024-05-16T15:34:47+08:00
featured: false
draft: false
comment: false
toc: false
reward: false
pinned: false
carousel: false
series:

categories:

tags: 
  - Unreal Engine 5.4
  - Unreal Engine
  - UE
authors:
  - Dexterowski
images: ["images/project-prepare-ue54.png"]
---

After writing the previous post, I finally managed to sit down and get some coding done on the project using Unreal Engine 5.4. As I mentioned earlier, I want to start from scratch and rewrite everything because the previous project was a huge mess with files and code, and I’ll also take this opportunity to upgrade the project from 5.3 to 5.4.

Initially, I installed all the necessary plugins I’ll be using and ensured they work with the latest Unreal Engine API. HammUEr required some fixes, but it eventually compiled. Once I confirmed the project is compiling, I could start working.

First, I created the classes for all the basic components, namely:

* Gamemode
* GameState
* PlayerState
* BasePlayer
* BasePlayerController
* BasePlayerHUD

I'm currently missing things like GameSession, Spectator etc. but I don't need them at the moment - I'm not sure if I want to manage Steam sessions from C++.

If I were making a single-player game, it wouldn’t matter where and what code I placed, but for a multiplayer game, the code should be placed in appropriate locations. The order of initialization of various objects and whether they exist on the client, the server, or both is also crucial.

**Gamemode** - As the name suggests, it should handle all the game rules. Here, you will find functions for restarting the game, respawning the player, etc. It exists only on the server, so it’s a safe place for managing such things. It inherits from the ``AGamemode`` class.

**GameState** -  As the name suggests, it stores our current game state and all necessary information. For example, the number of alive players, whether the round is ongoing, if the match has ended, who the points leader is, etc. All information here will be synchronized between server and clients. It inherits from the ``AGameState`` class.

<div class="alert alert-light"" role="alert">

I want most things on each map in my game to be configurable, such as how much health a player starts with, the maximum health they can have, whether they can sprint, if sprinting uses stamina, if they can use a flashlight, and many other rules. I created these rules in the gamemode class so that a derived gamemode with new rules for another map can be made. Then I am copying these set rules to the GameState object to ensure that every player has synchronized settings and to avoid complications of fetching values from Gamemode, which exists only on the server.

<br><br>

<center>

![Image Caption](gamerules.png "Example of coded gamerules which can be set on each map.")


</center>
</div>

**PlayerState** - As the name suggests, it stores information about the player, such as whether they are alive, how many points they have, etc. These values are transferred between map loads and do not reset when the player’s object is destroyed. Therefore, storing information like health here is not appropriate. It inherits from the ``APlayerState`` class.

**BasePlayer** - In Unreal, this class should simply be called "Character." I prefer the name I use because it’s more intuitive. This is where we place all such information like health. This class also handles the logic for taking damage, interacting with items, etc. It inherits from the ``ACharacter`` class.

**BasePlayerController** - This is our player’s controller, where we should place for example the logic for assigning key bindings to control the player. It inherits from the ``APlayerController`` class.

**BasePlayerHUD** - Here, we handle the entire player interface, including what should be displayed and when. It inherits from the ``AHUD`` class. Example:

```cpp
void ABasePlayerHUD::UpdateHealthHUD(float newHealth)
{
	UMainHUD* HUD = Cast<UMainHUD>(HUD_Widget);

	if(HUD)
	{
		HUD->UpdateHealth(newHealth);
	}
	else
	{
		GEngine->AddOnScreenDebugMessage(-1, 15, FColor::Red, "Can't get UMainHUD!");
	}
	
}
```
In my previous project, I had issues referring to these objects among themselves. Often, my code referred to GameState before it was initialized. The same issues applied to the player’s HUD. Therefore, I focused on ensuring that the code referred to this class in the correct place and that the HUD object was always available.

I also misused net-code previously. I struggled with understanding how it works in Unreal Engine, but it’s getting better. I realized that functions updating the player interface, like updating health upon spawn, need to be called only on the Owning Client. That's the reason, these functions must be RPCs (Remote Procedure Calls).

I created the ``RestorePlayer()`` function in the player class to restore default values like health and maximum health upon respawn.

```cpp
void ABasePlayer::RestorePlayer()
{
	// Get the values from gamerules/gamestate
	if(GameState)
	{
		UpdateHealth(GameState->StartingHealth);
	}
}
```

``UpdateHealth()`` body:

```cpp
void ABasePlayer::UpdateHealth(float newHealth)
{
	if(newHealth < 0)
		return;

	if(GameState)
	{
		if(newHealth >= GameState->MaxPlayerHealth)
		{
			Health = GameState->MaxPlayerHealth;
		}
		else
		{
			Health = newHealth;
			OnRep_HealthChanged();
		}
	}
		
	ClientUpdateHealth(newHealth);	
}
```

The code checks if health value is correct, checks if it is not greater than max health value set in gamerules. Then we set the health variable and execute RPC ``ClientUpdateHealth`` which looks like this:


```cpp
void ABasePlayer::ClientUpdateHealth_Implementation(float newHealth, bool bUpdateMaxHealth)
{
	ABasePlayerController* PlrController = Cast<ABasePlayerController>(GetController());
	if(PlrController)
	{
		PlrController->GetMainHUD()->UpdateHealthHUD(newHealth);
		if(bUpdateMaxHealth)
			PlrController->GetMainHUD()->UpdateMaxHealthHUD(GameState->MaxPlayerHealth);
	}
}
```
As you can see, we are looking for player controller which would have link to player HUD. Thanks to RPC we are sure that will be executed only on owning client - the player which are controlling this one particular player object.

{{% alert warning %}}

In Unreal Engine, it’s important to consider how to test multiplayer games. The best and most reliable way is to run the game using the **Standalone Game** option or uncheck `Run under one process` in the settings. This ensures the game runs as two separate processes. With this option checked, you might notice debug messages appearing on every client even though they are coded to display only on the Owning Client. This is not well-documented and I don't know why such a basic thing is not described anywhere.

<center>

![](standalonegame.png)

</center>

{{% /alert %}}

Here's what I've achieved so far on that clear 5.4 project:

![](hud.png)

The most important thing is that there are no issues with HUD initialization. Next, I plan to create an object that deals damage to our player, so player will be able to respawn afterward. I also need to create an actor for the spectator... a lot of work awaits me. I somewhat regret starting from scratch, but I know that doing it a second time will turn out better. It always turns out better. Does anyone else feel this way?