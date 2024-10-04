
## CUSTOM\_SELECT Folder

This example adds a tree view type control. Note that the folders and folder traversal are determined by the driver. There is nothing extra required in the action xml definition.

**1. Define the Action**

The following xml defines the action for CUSTOM\_SELECT Folder.


```xml
~<action>~
    <name>CUSTOM_SELECT Folder</name>
    <command>CUSTOM_SELECT_Folder</command>
    <sort_order>4</sort_order>
    <params>
        <param>
            <name>Select List Item</name>
            <type>CUSTOM_SELECT:GetFolderList</type>
        </param>
    </params>
</action>
```

In the example above:
The [Action Command][1] = CUSTOM\_SELECT\_Folder.  
The [Custom Select Function][2] = GetFolderList.

 **2. Implement the Custom Select Function**

_Implementation of GetFolderList()_

```lua
function GetFolderList(folderName)
    -- folderName = Genre

    local back = nil -- used to specify the list to return to when back button is selected.
    local list = {}  -- table of list items to return
    local isInFolder = not IsEmpty(folderName)

    if (isInFolder) then
        if folderName == 'rock' or folderName == 'oldies' or folderName == 'pop' or folderName == 'country' or folderName == 'jazz' then
            FilterByGenre(list, folderName, true)
            back = "" -- Back to the root folder
        else
            -- if there is a sub folder
            local subFolderIndex = string.find(folderName, '_')
            if subFolderIndex ~= nil then
                back = string.match(folderName, "(.*)_")
                for i = 1, 10 do
                    table.insert(list, { text = folderName .. " song " .. tostring(i), value = "song_" .. folderName .. "_" .. tostring(i) })
                end
            end
        end
    else
        -- at the root level
        InsertRootFolders(list)
    end

    return list, back
end
```

The GetFolderList function is provided for the "CUSTOM\_SELECT Folder" action to call when it needs the list of items available to select. The "CUSTOM\_SELECT Folder" action example is a tree view control of folders that contain the different list of genres. There are no filtering capabilities with this action. Items will be listed based on the folderName. If empty the root list will be provided.  

**Parameters**  
folderName: The name of the folder you are in. This will be empty the first time. In this example folderName equals a genre.  

**Return Values**  
list: A list of items based on the folderName, if empty the root level is returned.  
back: Name of the folder to return to when the back button is selected. If empty returns to the root level.


**3. Handle the Action Command**

All driver commands are handled in the ExecuteCommand function. To illustrate the handling of this action, we can update the "CUSTOM\_SELECT Action Search Result" property in the driver with the result of the action. Simply replace that code with whatever action is required for your driver’s needs.

_Handling of **CUSTOM\_SELECT\_Folder** action in Execute Command_

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
end
```


** ExecuteCommand Parameters**  
sCommand: Command sent to the driver. For actions, this will always be "LUA\_ACTION".  
tParams: Table of parameter key/value pairs for the command.

**Action Parameters**  
ACTION: The value of this item is the *[Action Command][3]* of the action executed. In this case it will be CUSTOM\_SELECT\_Folder.  
"Select List Item”: This parameter is the *[Parameter Name][4]* as defined in the xml. Its value is the value returned from the action.






[1]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options
[2]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-custom-select-function
[3]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options
[4]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options