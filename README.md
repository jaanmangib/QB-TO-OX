# QBCore to Ox Conversion Guide

This guide provides a complete step-by-step process to convert a QBCore-based script to work with Ox-related resources such as ox_core, ox_target, ox_inventory, and ox_lib.

## Requirements
Ensure you have the following Ox resources installed:
- **ox_core**: [GitHub](https://github.com/overextended/ox_core)
- **ox_target**: [GitHub](https://github.com/overextended/ox_target)
- **ox_inventory**: [GitHub](https://github.com/overextended/ox_inventory)
- **ox_lib**: [GitHub](https://github.com/overextended/ox_lib)

---

## 1. Converting QBCore Functions to ox_core

### Player Data Retrieval
Replace QBCore functions with Ox equivalents.

#### Before:
```lua
local Player = QBCore.Functions.GetPlayer(source)
local identifier = Player.PlayerData.citizenid
```
#### After:
```lua
local Player = exports.ox_core:GetPlayer(source)
local identifier = Player.charid
```

### Checking Player Jobs
#### Before:
```lua
if Player.PlayerData.job.name == "police" then
    -- Do something
end
```
#### After:
```lua
if Player.getGroup() == "police" then
    -- Do something
end
```

### Getting Money
#### Before:
```lua
local cash = Player.Functions.GetMoney("cash")
```
#### After:
```lua
local cash = Player.getAccount("cash").amount
```

### Adding/Removing Money
#### Before:
```lua
Player.Functions.AddMoney("bank", 500)
Player.Functions.RemoveMoney("cash", 100)
```
#### After:
```lua
Player.addAccountMoney("bank", 500)
Player.removeAccountMoney("cash", 100)
```

---

## 2. Replacing qb-target with ox_target
If your script uses `qb-target`, update it to `ox_target`.

#### Before:
```lua
exports['qb-target']:AddBoxZone("example", vector3(123.4, 567.8, 9.1), 1.5, 1.5, {
    name = "example",
    heading = 0,
    debugPoly = false,
    minZ = 8.9,
    maxZ = 10.5,
}, {
    options = {
        {
            type = "client",
            event = "example:event",
            icon = "fas fa-box",
            label = "Interact",
        }
    },
    distance = 2.0
})
```
#### After:
```lua
exports['ox_target']:addBoxZone({
    coords = vec3(123.4, 567.8, 9.1),
    size = vec3(1.5, 1.5, 1.6),
    rotation = 0,
    debug = false,
    options = {
        {
            event = "example:event",
            icon = "fas fa-box",
            label = "Interact",
        }
    }
})
```

---

## 3. Updating qb-menu to ox_lib Context Menus

If your script uses `qb-menu`, switch to `ox_lib`'s context menu system.

#### Before:
```lua
exports['qb-menu']:openMenu({
    {
        header = "Main Menu",
        isMenuHeader = true
    },
    {
        header = "Option 1",
        txt = "Do something",
        params = {
            event = "example:event"
        }
    }
})
```
#### After:
```lua
lib.registerContext({
    id = 'main_menu',
    title = "Main Menu",
    options = {
        {
            title = "Option 1",
            description = "Do something",
            event = "example:event"
        }
    }
})
lib.showContext("main_menu")
```

---

## 4. Replacing qb-inventory with ox_inventory

### Adding Items
#### Before:
```lua
Player.Functions.AddItem("example_item", 1)
TriggerClientEvent("inventory:client:ItemBox", source, QBCore.Shared.Items["example_item"], "add")
```
#### After:
```lua
exports['ox_inventory']:AddItem(source, "example_item", 1)
```

### Removing Items
#### Before:
```lua
Player.Functions.RemoveItem("example_item", 1)
```
#### After:
```lua
exports['ox_inventory']:RemoveItem(source, "example_item", 1)
```

### Checking Item Count
#### Before:
```lua
local itemCount = Player.Functions.GetItemByName("example_item").amount
```
#### After:
```lua
local itemCount = exports['ox_inventory']:GetItem(source, "example_item", false)
```

---

## 5. Using ox_lib Notifications
If your script uses `QBCore.Functions.Notify`, switch to `ox_lib` notifications.

#### Before:
```lua
QBCore.Functions.Notify("This is a message", "success")
```
#### After:
```lua
lib.notify({
    title = "Notification",
    description = "This is a message",
    type = "success"
})
```

---

## Conclusion
This resource should help you understand how to make qbcore scripts compatible with ox_core.
If you find or want to add qbcore or oxcore exports, open a pull request and make your changes there
