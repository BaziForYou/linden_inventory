---
title: Introduction
---

<h2 align='center'> Requirements </h2>


* ESX Legacy [[Download]](https://github.com/esx-framework/es_extended/tree/legacy)
* GHMattiMySQL [[Download]](https://github.com/GHMatti/ghmattimysql/releases)
* Mythic Notify [[Download]](https://github.com/thelindat/mythic_notify)
* Mythic Progbar [[Download]](https://github.com/thelindat/mythic_progbar)
* OneSync must be enabled (up to 32 slots is free)
* The ability to follow instructions and learn  


<h2 align='center'> Server Config </h2>

```lua
set mysql_connection_string "mysql://user:password@localhost/database?connectTimeout=30000&acquireTimeout=30000&waitForConnections=true&keepAlive=30&charset=utf8mb4"
set onesync legacy		# do not use infinity unless you know what you're doing
#set sv_enforceGameBuild 2060	# enable Los Santos Summer Special build

add_ace resource.es_extended command.add_ace allow
add_ace resource.es_extended command.add_principal allow
add_ace resource.es_extended command.remove_principal allow
add_ace resource.es_extended command.stop allow

ensure mapmanager
ensure chat
ensure spawnmanager
ensure sessionmanager
ensure hardcap

ensure mysql-async
ensure ghmattimysql
ensure cron
ensure es_extended

ensure esx_menu_default
ensure esx_menu_list
ensure esx_menu_dialog

ensure skinchanger
ensure esx_skin
ensure esx_identity
#
#
#
#
#
ensure linden_inventory		# load after resources that register items, or just last
```


<h2 align='center'> Modification </h2>

### ghmattimysql
* Delete `config.json` to fallback to using the MySQL connection string in server.cfg
* Add the following code to ghmattimysql-server.lua
```lua
exports("ready", function (callback)
	Citizen.CreateThread(function ()
		while GetResourceState('ghmattimysql') ~= 'started' do
			Citizen.Wait(0)
		end
		callback()
	end)
end)
```


<h2 align='center'> Framework </h2>


ESX 1.1 (running essentials) has _never_ been compatible, and if you were planning on running it then I don't recommend you bother trying to use this inventory (or running a server, for that matter).  
As of version 1.5.0 you are required to use ESX Legacy to use this resource.


You may, optionally, download [my fork of ESX Legacy](https://github.com/thelindat/es_extended/) which has all the changes to use this inventory in place, on top of a few extra features.


For a guide on modifying your framework, go to [this page](framework).


<h2 align='center'> Upgrading </h2>


If you are upgrading from a prior version of ESX or EXM you may be using steam as your primary identifier.  
This is not recommended by ESX or CFX but if it is absolutely necessary you can modify your framework to support it.  
Look through ESX's server files for references to `license` and change them where appropriate.