--- 
title: Custom Movement - Sprint and Stamina System
date: 2024-06-06T15:34:47+08:00
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
  - C++
  - CPP
  - Movement
  - Stamina
  - Network Prediction
  - Replication
authors:
  - Dexterowski

images: ["images/sprint.png"]
---

That's the thing I don't know how to handle because it must be predicted perfectly to gain smooth gameplay. Making custom movement would be easy for a singleplayer game, but things get a lot of more complicated in multiplayer - You won't do this in **Blueprints**. This is the first and basic limitation of visual programming in UE5. That's the reason I've switched to code my game in C++. Firstly, I must have custom movement for my players. Secondly, I want to learn C++ and UE5 API.

But UE5 has movement component which contains swimming, jumping, running, crouching, flying? 
Yes it has, but no any more. I need things such as:

* Sprinting
* Ladder climbing (Half-Life 1 System and HL2 as separate entites/components)
* Smooth crouching(?)
* Water swimming, water wade walking, water shallow walking 

I need two more specific types for water movement because:

**Water Wade Walking** is needed for lowering amount of speed player can gain and limit the jump height. Also we will need to make proper sounds and particle effects and that's the way how I will handle it.

**Water Shallow Walking** same as above but here I only need to determine the footsteps sound and particle effects(*maybe I forgot about something*)

So let's make ``DXMovementComponent``. I've made in my old project before starting the new one sprinting system which worked with stamina and network prediction also worked perfectly

{{% alert context="info" %}}

**Network prediction** in movement is very important, because while one player have very poor network connection for example (400ms ping) for him it would be still a smooth experience without any lag, jitter

{{% /alert %}}

I will not describe here what am I doing because I do not have any idea. This is a DevLog, not tutorial xD

I have my code done in movement component, the player is sprinting. It works, it is not jittery when ping is high and packet loss is also high it's correctly predicted. I just copied someone's code from a tutorial. It works? Works xD For a base sprinting ability it is good. I think it would be a good "base code" to make another custom movement things.

## Player friction

Additionaly I coded changing friction to a player when he runs for example on ice.  I did it wrong in case of performance because it's checking the material type every tick(?).

```cpp

void UDXMovementComponent::UpdateCharacterStateBeforeMovement(float DeltaSeconds)
{
	Super::UpdateCharacterStateBeforeMovement(DeltaSeconds);

	if (CharacterOwner->GetLocalRole() != ROLE_SimulatedProxy)
	{
		// Sprinting
		if (CanSprint())
			bIsSprinting = true;
		else
			bIsSprinting = false;
		
		FHitResult OutHit;
		FVector StartLocation = GetOwner()->GetActorLocation();
		FVector EndLocation = StartLocation - FVector(0.f, 0.f, 100.f);

		FCollisionQueryParams TraceParams;
		TraceParams.bTraceComplex = true; 
		TraceParams.bReturnPhysicalMaterial = true; 
		TraceParams.AddIgnoredActor(GetOwner()); 

		
		if (GetWorld()->LineTraceSingleByChannel(
			OutHit,
			StartLocation,
			EndLocation,
			ECC_Visibility, // Użyj odpowiedniego kanału kolizji dla podłogi
			TraceParams
		))
		{
			UPhysicalMaterial* HitPhysicalMaterial = OutHit.PhysMaterial.Get();
			FString MaterialName = HitPhysicalMaterial->GetName();

			UpdateFriction(Cast<UCorePhysMaterial>(HitPhysicalMaterial));
			
		}
	
	}
}



// Can Pass NULLPTR PARAMETER!!
void UDXMovementComponent::UpdateFriction(UCorePhysMaterial* PhysMaterial)
{
	if (CVarDisplayMovementInfo->GetInt() > 0 && PhysMaterial)
		GEngine->AddOnScreenDebugMessage(18, 5, FColor::White, "PhysMaterial: " + PhysMaterial->GetName() + "Braking Deceleration Speed:" + FString::SanitizeFloat(BrakingDecelerationWalking));

	if (PhysMaterial)
	{
		// Set friction based on material
		GroundFriction = PhysMaterial->Friction;
		BrakingDecelerationWalking = PhysMaterial->Friction * 256;
	}
	else
	{
		// Set default friction
		GroundFriction = 8.0f;
		BrakingDecelerationWalking = 2048;
	}
}

```

As you can see I also added a [console command]({{< ref "/docs/console-commands">}}) called ``cl_DisplayMovementInfo`` which will print out debug things like these you can see in the code block. *(yeah I stoled the command naming convention from GoldSrc/Source engine xD because it is easier to remember!)*

``` none
cl_* - Client command prefix
sv_* - Sever command prefix
r_* - Render command prefix (It is also used by UE if I am not wrong)
etc.
```

I will try to stick to these.

*I am not sure if changing friction should be predicted as well, but when I tested it it looks good*

## Stamina

When I has working sprint, now it would be a great thing to limit it, huh? I think so - Somebody who will make a map can also decide if player should use stamina or not, how fast it should be decreasing, regenerating and many more.

<center>

![](https://i.imgur.com/jj6SBjH.png "Available Stamina settings in Gamerules")

</center>

I coded something and it not works. Logically stamina shouldbe only calculated on the server and updated also only on the server, client must need only **REQUEST** from the server calculation

```cpp

void ABasePlayer::CalcStamina(float DeltaTime)
{
	Stamina_TimeSinceLastUpdate += DeltaTime;

	if (Stamina_TimeSinceLastUpdate >= 0.1f)
	{
		Stamina_TimeSinceLastUpdate = 0.0f;

		UpdateStamina(); // ServerRPC
	}
}
```
So I did it, server should calc every 0.1s new stamina value. 

```cpp

void ABasePlayer::UpdateStamina_Implementation()
{

	if (DXBaseMovementComponent->Velocity.Length() > DXBaseMovementComponent->MaxWalkSpeed * 0.5 &&DXBaseMovementComponent->IsMovingForward() && DXBaseMovementComponent->IsMovingOnGround() && DXBaseMovementComponent->bWantsToSprint)
	{
		const float NewStamina = FMath::Clamp(Stamina - GameState->PlayerStaminaDecreaseFactor * GetWorld()->DeltaTimeSeconds, 0.0f, MaxStamina);


		if (CVarDisplayStaminaInfo->GetInt() > 0)
		{
			GEngine->AddOnScreenDebugMessage(52, 1.0f, FColor::White, FString::Printf(TEXT("Server StaminaVar: %f, Decrease Factor: %f"), NewStamina, GameState->PlayerStaminaDecreaseFactor));
		}

		Stamina = NewStamina;
	}
}

```

I also did an interpolation for a stamina bar for clients but it doesn't matter right now. The problem with this code is: When we are playing as **client** it is calculated correctly. When we play as **Listen Server** it's get slower. It is not decreasing in the same speed as in clients but slower. Wtf?

<hr>

I asked for help on Unreal Source discord and they told me to make it in ``CharacterMovementComponent`` because it affects the movement and should be also **predicted**. It crossed my mind but I though I am wrong and I wasn't.

<center>

<video controls src="UnrealEditor_OGBMoRdCRz.mp4" title="Working predicted stamina" width="100%"></video>

</center>

I coded it in MovementComponent and made somehow (don't know how the fuck it works) prediction for it and it WORKS AT 200-400ms* ping. No position corrections at all, so it looks I think.. good?

So above code in ``ABasePlayer`` class is removed completly.

*\*nobody has that trash internet in novadays* 

Second perspective from another **client**:

<center>

<video controls src="UnrealEditor_ArAHmY8S68.mp4" title="Other perspective" width="100%"></video>

and **SERVER**

<video controls src="UnrealEditor_dc6hPveV8j.mp4" title="Server perspective stamina" width="100%"></video>

</center>

So as we can see the corrections are visible but I think it is still pretty good for such a high ping.

Ok let's make the rest, we need regenereting and UI representation of stamina as you can see it is not working yet.


<center>

![](hudstaminabar.gif)

</center>

But it is getting updated in ``ABasePlayer::Tick`` function which is not so good place to call it. It is messy.

```cpp

void ABasePlayer::UpdateHUD()
{
	// Stamina
	// Do not update it every tick. Do it only when Player is sprinting, regenerating stamina etc...
	if (IsLocallyControlled() && DXBaseMovementComponent->bIsSprinting)
	{
		ABasePlayerController* PlrController = Cast<ABasePlayerController>(GetController());
		if (PlrController && PlrController->GetMainHUD())
		{
			PlrController->GetMainHUD()->UpdateStaminaHUD(DXBaseMovementComponent->Stamina, GameState->PlayerMaxStamina);
		}
	}
}

``` 
I made just a separate function for it. Now it's the time to make regenerating the stamina. It should be easy but I am wondering if it should be also done in **Movementcomponent** or maybe **ABasePlayer**? I think if I coded stamina drain system in **MovementComponent** I should done everything related to stamina there.

<hr>

Ok it is next day and I did stamina regenereting and... it just works as it should. Lol. 

<center>

{{< video src="UnrealEditor_b0EOCfeCa1.mp4" autoplay="true" controls="false" loop="true" width="100%" muted="true" >}}

</center>

All I need to do is update player HUD when spawns. As it turns out this is the biggest problem. Always "First Player Spawn" things are hard to handle because of desynchronization. Listen Server spawns instantly and have initialized whole actors like GameState, PlayerState etc. Client joins and have initialized NOTHING. We must wait for initialization - I can't use timer to make a delay to wait for initializing things. This is a very **bad practice**. I must find proper place to code it.

Ok I found the place, I forgot I made a function for this:

```cpp

void ABasePlayer::RestorePlayer()
{

	// Get the values from gamerules/gamestate
	if(GameState)
	{
		UpdateHealth(GameState->StartingHealth);
		ClientUpdateStamina(GameState->PlayerMaxStamina);
		DXBaseMovementComponent->Stamina = GameState->PlayerMaxStamina;
	}

	if(BasePlayerState)
	{
		BasePlayerState->IsAlive = true; // should be only called by server.
//		BasePlayerState->OnRep_IsAlive();
	}
}

```

It is being called by ``ABasePlayer::PossesedBy``  and ClientRPC looks like this:

```cpp

void ABasePlayer::ClientUpdateStamina_Implementation(float Stamina)
{
	ABasePlayerController* PlrController = Cast<ABasePlayerController>(GetController());
	if (PlrController)
	{
		PlrController->GetMainHUD()->UpdateStaminaHUD(Stamina, Stamina);
	}
}

```

So it works, it is fully configurable from gamerules, it is draning it is regenerating. Nice.


**I forgot about one feature: **

![Player Jump Stamina Drain Amount Config](image.png)

I need implement this also, and function for this will come handy for setting variable for determining **player model animations** but we will do it in a while...

<center>

{{< video src="UnrealEditor_3pmxo53dMM.mp4" autoplay="true" controls="false" loop="true" width="100%" muted="true" >}}

</center>


ahhh, I forgot to reset time to the stamina regeneration, jumping drains the stamina but counter for ability to regenerate stamina is not resetting. One minor change and it will be good.


**From this:**

```cpp
bool UDXMovementComponent::DoJump(bool bReplayingMoves)
{

	if(bIsSprinting)
	{
		Stamina = FMath::Clamp(Stamina - GameState->PlayerJumpStaminaDrainAmount, 0.0f, GameState->PlayerMaxStamina);
	}
[...]
```

**To this:**

```cpp
bool UDXMovementComponent::DoJump(bool bReplayingMoves)
{

	if(bIsSprinting)
	{
		Stamina = FMath::Clamp(Stamina - GameState->PlayerJumpStaminaDrainAmount, 0.0f, GameState->PlayerMaxStamina);
		StaminaLastUsedTime = GameState->GetServerWorldTimeSeconds();
		bStaminaCanRegenerate = false;
	}
[...]
```

This code repeats for draining the stamina, I should refactor this and make a separate function to do it. I will do that later... :upside_down_face:

I should do it now, because when I will code more caess when stamina is drained for example swimming in water it will annoying.

**And another bug:**

When player is WALKING with speed for example 10 of 640 (full run speed without sprinting), holds sprint key(ex. shift) and jumps stamina is draining. This will also require small change in code. As you can guess wwe must change the condition in our jumping function - because it is not checking if we have enough speed to be "sprinting and jumping". 

{{% alert context="info" %}}

Stamina drains only when player is running about his walking speed (640cm/s for now)

{{% /alert %}}

For this condition I also should shorten this to individual function...

```cpp
	// It checks if player is  ACTUALLY SPRINTING not the same as bIsSprinting, it checks if player is moving faster than MaxWalkSpeed - in that case stamina also starts to drain

	FORCEINLINE bool IsActuallySprinting() const { if (bIsSprinting && (Velocity.Size() > MaxWalkSpeed)) return true; return false; };
```


<center>


**Before change:**

<video src="UnrealEditor_jQ9WpEHhfK.mp4" autoplay="true" title="Stamina draining jumping BUG" width="100%" controls="false" loop="true" ></video>
*I am holding sprint key whole time*


**After change:**

<video src="UnrealEditor_bqOtEX1y6F.mp4" autoplay="true" title="Stamina draining fix" controls="false" loop="true" width="100%" ></video>
*I am holding sprint key whole time*


</center>

## HUD cosmetic change

I don't think that stamina bar should be always visible. I think health bar is enough. Let's make it visible only when it is in use.

Simple but I think nice:

<video controls src="UnrealEditor_lvJoQaImb5.mp4" title="Stamina HUD hiding animation"  width="100%"></video>

but it also has bug xD


<video controls src="HUDStaminaFadeOut_Bug.mp4" title="Stamina HUD hiding animation BUG"  width="100%"></video>


Fixed this by stopping the animation if one is currently playing:

```cpp
void UMainHUD::UpdateStamina(float NewStamina, float NewMaxStamina)
{
	int IntStamina = static_cast<int32>(NewStamina);
	CalcStaminaProgressBar(NewStamina, NewMaxStamina);

	if (bIsStaminaOut)
	{
		StaminaBox->SetRenderOpacity(1);
		StopAnimation(StaminaOutAnim);
		bIsStaminaOut = false;
	}
	LastTimeStaminaUsed = GetWorld()->GetTimeSeconds();
}
```

I don't know if this is the right solution but... works? Works.

Also made the [console variable]({{< ref "/docs/console-commands">}}) which can adjust the time before fading out the stamina bar

``` cl_HUD_StaminaOutDelay ```