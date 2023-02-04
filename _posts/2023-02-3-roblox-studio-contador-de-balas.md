---
layout: post
title:  "Cómo agregar un contador de balas en Roblox Studio"
description: Cómo agregar un contador de balas en Roblox Studio
comments: true
category: Roblox
tags: Tutoriales Trucos
youtube: https://youtu.be/EbKw0Dcaf6o
---
Codigo para agregar un contador de balas en Roblox Studio usando el (ACS) Advanced Combat System.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:

Editar linea 1511 en StarterPlayer/StarterCharacterScripts/ACS_Client/ACS_Framework
```C#
	if WeaponData.ShootType == 1 then
		HUD.FText.Text = Ammo.."/"..StoredAmmo.." | Semi"
	elseif WeaponData.ShootType == 2 then
		HUD.FText.Text = Ammo.."/"..StoredAmmo.." | Burst"
	elseif WeaponData.ShootType == 3 then
		HUD.FText.Text = Ammo.."/"..StoredAmmo.." | Auto"
	elseif WeaponData.ShootType == 4 then
		HUD.FText.Text = Ammo.."/"..StoredAmmo.." | Pump-Action"
	elseif WeaponData.ShootType == 5 then
		HUD.FText.Text = Ammo.."/"..StoredAmmo.." | Bolt-Action"
	end
```
