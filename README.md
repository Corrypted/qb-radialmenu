# qb-radialmenu
Radial Menu Used With QB-Core :arrows_counterclockwise:

**READ**
You will need to put this snippet in your qb-policejob :)
```lua
-- Client Side
RegisterNetEvent('police:client:hijack', function()
    local cop = PlayerPedId()
    local copcoords = GetEntityCoords(cop)
    local vehicle = QBCore.Functions.GetClosestVehicle()
    local vehiclepos = GetEntityCoords(vehicle)
    local PlayerJob = QBCore.Functions.GetPlayerData().job
    
    if #(copcoords - vehiclepos) < 3.0 then
        if GetVehicleDoorLockStatus(vehicle) == 0 then QBCore.Functions.Notify("This vehicle doesn't seem to be locked.", "error") return end
        if PlayerJob.type == 'leo' then
            TriggerEvent('animations:client:EmoteCommandStart', {"weld"})
            QBCore.Functions.Progressbar("policeunlock", "Unlocking vehicle..", 5000, false, false, {
                disableMovement = true,
                disableCarMovement = true,
                disableMouse = false,
                disableCombat = true,
                }, {}, {}, {}, function()
                TriggerEvent('animations:client:EmoteCommandStart', {"weld"})
                Wait(100)
                TriggerEvent('animations:client:EmoteCommandStart', {"c"})
                Wait(500)
                TriggerServerEvent("InteractSound_SV:PlayWithinDistance", 5, "lock", 0.3)
                QBCore.Functions.Notify('Vehicle unlocked.', 'success')
                TriggerServerEvent('qb-vehiclekeys:server:setVehLockState', NetworkGetNetworkIdFromEntity(vehicle), 1)
                TriggerServerEvent('qb-vehiclekeys:server:AcquireVehicleKeys', QBCore.Functions.GetPlate(vehicle))
                SetVehicleAlarm(vehicle, false)
            end)
        else
            QBCore.Functions.Notify("You are not Police!", "error")
        end
    else
        QBCore.Functions.Notify("Not near any vehicle.", "error")
    end
end)
```

```lua
-- Server Side (Optional) '/' Command
QBCore.Commands.Add("pdunlock", "Unlock closest vehicle (PD)", {}, false, function(source)
    local src = source
    local Player = QBCore.Functions.GetPlayer(src)
    if Player.PlayerData.job.type == "leo" then
        TriggerClientEvent("police:client:hijack", src)
    else
        TriggerClientEvent('QBCore:Notify', src, "PD Only", 'error')
    end
end)
```
**Now using FontAwesome Icons!**
To change icons get the name from [FontAwesome](https://fontawesome.com/v5.0/icons?d=gallery&p=2&s=brands,light,regular,solid&m=free) (v5) and use the icon's name in the config.lua for the icon (no `fa-` or `#` just the name like `arrow-right`)

# License

    QBCore Framework
    Copyright (C) 2021 Joshua Eger

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <https://www.gnu.org/licenses/>
