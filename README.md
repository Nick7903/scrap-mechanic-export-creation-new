# scrap-mechanic-export-creation-new

This will show a clumsy, but working, way of exporting and importing Scrap Mechanic creations in survival, after the export command was removed.

D:\SteamLibrary\steamapps\common\Scrap Mechanic\Survival\LocalBlueprints
D:\SteamLibrary\steamapps\common\Scrap Mechanic\Survival\Logs
D:\SteamLibrary\steamapps\common\Scrap Mechanic\Survival\Scripts\game\SurvivalGame.lua

1.  open ..\steamapps\common\Scrap Mechanic\Survival\Scripts\game\SurvivalGame.lua
2.  search for, and find; `sm.game.bindChatCommand( "/import"` about at line 217
3.  above that line add the following new line
   ```
	sm.game.bindChatCommand( "/exportbylog", {}, "cl_onChatCommand", "Exports blueprint by writing it to log" )`
```

6.  search for, and find; `elseif params[1] == "/import" then` about at line 546
7.  above it add the following code
   ```
	elseif params[1] == "/exportbylog" then
		local rayCastValid, rayCastResult = sm.localPlayer.getRaycast( 100 )
		if rayCastValid and rayCastResult.type == "body" then
			local exportbylogParams = {
				body = rayCastResult:getBody()
			}
			self.network:sendToServer( "sv_exportbylogCreation", exportbylogParams )
		end
```

6.  search for, and find; `function SurvivalGame.sv_importCreation( self, params )` about at line 763
7.  above it add the following code
   ```
	function SurvivalGame.sv_exportbylogCreation( self, params )
		sm.log.warning( sm.creation.exportToString( params.body ) )
	end
```
