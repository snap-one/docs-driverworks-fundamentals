
## CUSTOM\_SELECT Search

This example provides a search field in the Composer Pro’s CUSTOM\_SELECT UI. In our example, the box accepts string based searches and returns matching data from the `g_tMusicGenre` table.

**1. Define the Action**

The following xml defines the action for CUSTOM\_SELECT Search.

```xml
<action>
    <name>CUSTOM_SELECT Search</name>
    <command>CUSTOM_SELECT_Search</command>
    <sort_order>2</sort_order>
    <params>
        <param>
            <name>Select List Item</name>
            <type>CUSTOM_SELECT:GetSearchList</type>
            <search />
        </param>
    </params>
</action>
```

In the example above:

The [Action Command][1] = CUSTOM\_SELECT\_Search.  
The [Custom Select Function][2] = GetSearchList.  
The \<search /\> tag enables the search box in the `Select` diablog.


 **2. Implement the Custom Select Function**

_Implementation of GetSearchList()_

```lua
function GetSearchList(folderName, callback, searchText)
    local list = {} -- table of list items to return

    if IsEmpty(searchText) then
        InsertAll(list)
    else
        FilterText(list, searchText)
    end

    return (list)
end
```


The GetSearchList function is provided for the "CUSTOM\_SELECT Search" action to call when it needs the list of items available to select. The "CUSTOM\_SELECT Search" action example is a flat list with filtering by a string. If searchText is empty, the full list is returned, Otherwise, only items that contain the search text will be returned


**3. Handle the Action Command**

All driver commands are handled in the ExecuteCommand function. To illustrate the handling of this [action command][3], we update the "CUSTOM\_SELECT Action Search Result" property in the driver with the result of the action. Simply replace that code with whatever action is required for your driver’s needs.

_Handling of **CUSTOM\_SELECT\_Search** action in Execute Command_

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
end
```


** ExecuteCommand Parameters**  
sCommand: Command sent to the driver. For actions, this will always be "LUA\_ACTION".  
tParams: Table of parameter key/value pairs for the command.

**Action Parameters**  
ACTION: The value of this item is the *[Action Command][4]* of the action executed. In this case it will be CUSTOM\_SELECT\_Search.  
"Select List Item": This parameter is the *[Parameter Name][5]* as defined in the xml. Its value is the value returned from the action.


[1]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options
[2]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-custom-select-function
[3]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options
[4]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options
[5]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options