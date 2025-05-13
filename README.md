# scrap-mechanic-export-creation-new

This will show a clumsy, but working, way of exporting and importing Scrap Mechanic creations in survival, after the export command was removed.

This only works if you are the host of the world.

1.  open `..\steamapps\common\Scrap Mechanic\Survival\Scripts\game\SurvivalGame.lua`
2.  make sure you have access to commands, by adding `g_survivalDev = true` above the first function about at line 40
   
3.  search for, and find; `sm.game.bindChatCommand( "/import"` about at line 217
4.  above that line add the following new line
   ```
	sm.game.bindChatCommand( "/exportbylog", {}, "cl_onChatCommand", "Exports blueprint by writing it to log" )
```

5.  search for, and find; `elseif params[1] == "/import" then` about at line 546
6.  above it add the following code
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

7.  search for, and find; `function SurvivalGame.sv_importCreation( self, params )` about at line 763
8.  above it add the following code
   ```
	function SurvivalGame.sv_exportbylogCreation( self, params )
		sm.log.warning( sm.creation.exportToString( params.body ) )
		self.network:sendToClients( "client_showMessage", "Exported creation to log file" )
	end
```

9. save the file

10. exit Scrap Mechanic world and reenter, no need to restart game

11. go up to and point crosshair at free floating creation ( not on lift, not connected to world )
12. press enter to type command, type "/exportbylog", make sure it responds with "Exported creation to log file"

13. find the newest log file in `..\steamapps\common\Scrap Mechanic\Logs` with name starting with "game-" ( NOT "mygui-" )
14. search the file for `wrap_Log.cpp:26`, and copy the rest of the line starting with where it says `{"bodies":[`

15. paste the text into a new file in `..\steamapps\common\Scrap Mechanic\Survival\LocalBlueprints` with the extension `.blueprint`
16. assert that you have a file in the `localBlueprints` folder, with a name like `mycreation.blueprint` with text in it that starts with `{"bodies":[` and ends with `,"version":4}`

17. enter any Scrap Mechanic world and use the import command to import your creation: `/import mycreation`
