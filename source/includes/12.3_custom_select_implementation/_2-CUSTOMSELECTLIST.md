
## CUSTOM\_SELECT List

This example provides a simple, flat list with no search or filtering functionality.

**1. Define the Action**

The following xml defines the action for CUSTOM\_SELECT List.

```xml
~<actions>~
    <action>
        <name>CUSTOM_SELECT List</name>
        <command>CUSTOM_SELECT_List</command>
        <sort_order>1</sort_order>
        <params>
            <param>
                <name>Select List Item</name>
                <type>CUSTOM_SELECT:GetList</type>
            </param>
        </params>
    </action>
```

In the example above:

The [Action Command][1] = CUSTOM\_SELECT\_List.  
The [Custom Select Function][2] = GetList.

 **2. Implement the Custom Select Function**

_Implementation of GetList()_

```lua
function GetList()
    local list = {} -- table of list items to return

    InsertAll(list)

    return (list)
end
```

In this example we want to return the full list of data from the `g_tMusicGenre` table . Since there is no search, filtering or folder functionality, the GetList function needs no parameters and simply returns the full list in the table. In our sample driver, this function is implemented in the drivers’ sample\_custom\_select.lua file.


 **3. Handle the Action Command**
All driver commands are handled in the ExecuteCommand function. To illustrate the handling of this [action command][3], we can update the "CUSTOM\_SELECT Action List Result" property in the driver with the result of the action. Simply replace that code with whatever action is required for your driver’s needs.

_Handling of **CUSTOM\_SELECT\_List** action in Execute Command_

```lua
function ExecuteCommand(sCommand, tParams)
local selectedItem = ""

if sCommand == "LUA_ACTION" then
    if (tParams ~= nil) then
        local action = tParams["ACTION"]
        if action == "CUSTOM_SELECT_List" then
            selectedItem = tParams["Select List Item"]
            C4:UpdateProperty("CUSTOM_SELECT Action List Result", tostring(selectedItem))
        elseif action == "CUSTOM_SELECT_Search" then
            selectedItem = tParams["Select List Item"]
            C4:UpdateProperty("CUSTOM_SELECT Action Search Result", tostring(selectedItem))
        elseif action == "CUSTOM_SELECT_Filter" then
            selectedItem = tParams["Select Channel"]
            C4:UpdateProperty("CUSTOM_SELECT Action Filter Result", tostring(selectedItem))
        elseif action == "CUSTOM_SELECT_Folder" then
            selectedItem = tParams["Select List Item"]
            C4:UpdateProperty("CUSTOM_SELECT Action Folder Result", tostring(selectedItem))
        elseif action == "CUSTOM_SELECT_Async" then
            selectedItem = tParams["Select List Item"]
            C4:UpdateProperty("CUSTOM_SELECT Action Async Result", tostring(selectedItem))
        elseif action == "CUSTOM_SELECT_Async_Country" then
            selectedItem = tParams["Select List Item"]
            C4:UpdateProperty("CUSTOM_SELECT Action Async2 Result", tostring(selectedItem))
        elseif action == "CUSTOM_SELECT_All" then
            selectedItem = tParams["Select List Item"]
            C4:UpdateProperty("CUSTOM_SELECT Action All Result", tostring(selectedItem))
        elseif action == "CUSTOM_SELECT_Timer" then
            selectedItem = tParams["Select List Item"]
            C4:UpdateProperty("CUSTOM_SELECT Action Async Timer Result", tostring(selectedItem))
        else
            print ("Action not found: " .. action)
        end
    end
```


** ExecuteCommand Parameters**  
sCommand: Command sent to the driver. For actions, this will always be "LUA\_ACTION".  
tParams: Table of parameter key/value pairs for the command.

**Action Parameters**  
ACTION: The value of this item is the *[Action Command][4]* of the action executed. In this case it will be CUSTOM\_SELECT\_List.  
"Select List Item": This parameter is the *[Parameter Name][5]* as defined in the xml. Its value is the value returned from the action.




[1]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options
[2]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-custom-select-function
[3]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options
[4]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options
[5]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options