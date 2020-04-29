> ⚠️ This script is currently broken. Thanks for the next esx updates that kill backwards compatibility.

> ♻️ If someone has time to debug, they can create a pull request with fix.

> 💻 You can download es_extended here: https://gitlab.com/TigoDevelopment/es_extended
___
# Thanks to KASH and XxFri3ndlyxX
> If you are updating ESX, be sure to update **all scripts** and **DATABASE SCHEMA**!

Instrukcja w języku Polskim znajduje się [tutaj](https://github.com/fivem-ex/esx_kashacter/blob/master/readme-pl.md).

## Required changes:

* es_extended: (`es_extended/client/main.lua`)

### Remove this code (74 - 76):
```lua
Citizen.Wait(3000)
ShutdownLoadingScreen()
DoScreenFadeIn(10000)
```

* es_extended: (`es_extended/client/main.lua`)

### Replace this code:

```lua
Citizen.CreateThread(function()
	while true do
		Citizen.Wait(0)

		if NetworkIsPlayerActive(PlayerId()) then
			TriggerServerEvent('esx:onPlayerJoined')
			break
		end
	end
end)
```

### with:

```lua
RegisterNetEvent('esx:kashloaded')
AddEventHandler('esx:kashloaded', function()
	TriggerServerEvent('esx:onPlayerJoined')
end)
```

* es_extended: (`es_extended/server/main.lua`)

### Change this code in `onPlayerJoined(playerId)` function:
### also remember that there are 2 places where the code below is written:
```lua
	for k,v in ipairs(GetPlayerIdentifiers(playerId)) do
		if string.match(v, 'license:') then
			identifier = string.sub(v, 9)
			break
		end
	end
```

### remember to replace both of them with:
```lua
	for k,v in ipairs(GetPlayerIdentifiers(playerId)) do
		if string.match(v, 'license:') then
			identifier = v
			break
		end
	end
```
___

# Read carefully...
> You **MUST** increase the character limit in the `users` table for row `identifier` to **48**.

> Do **not** use essentialsmode, mapmanager and spawnmanager!

> *Pay ATTENTION: You have to call the resource **esx_kashacter** in order for the javascript to work!**

___

## How it works
> What this script does it manipulates ESX for loading characters
So when you are choosing your character it changes your **Rockstar license** which is normally **license:** to **Char:** this prevents ESX from loading another character because it is looking for you exact license. So when you choose your character it will change it from Char: to your normal Rockstar license (license:). When creating a new character it will spawn you without an exact license which creates a new database entry for your license.

## Multiple languages support
Just change locales/en.js in html/ui.html (line 10)
