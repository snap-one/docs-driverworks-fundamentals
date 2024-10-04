## CUSTOM\_SELECT All

This example demonstrates the implementation of several CUSTOM\_SELECT features within one Action. It includes List, Search, Filter, Folder.


**1. Define the Action**

The following xml defines the action for CUSTOM\_SELECT All.

```xml
<action>
    <name>CUSTOM_SELECT All</name>
    <command>CUSTOM_SELECT_All</command>
    <sort_order>8</sort_order>
    <params>
        <param>
            <name>Select List Item</name>
            <type>CUSTOM_SELECT:GetListAll</type>
            <search>
                <filters>
                    <filter name="all">All</filter>
                    <filter name="50s">50s</filter>
                    <filter name="60s">60s</filter>
                    <filter name="70s">70s</filter>
                    <filter name="80s">80s</filter>
                    <filter name="90s">90s</filter>
                    <filter name="2k">90s</filter>
                </filters>
            </search>
        </param>
    </params>
</action>
```

In the example above:  
The [Action Command ][1]= CUSTOM\_SELECT\_All.  
The [Custom Select Function][2] = GetListAll.


 **2. Implement the Custom Select Function**

_Implementation of GetListAll()_

```lua
function GetListAll(folderName, callback, searchText, filter)
    -- folderName = Genre
    -- filter = period

    local back = nil -- used to specify the list to return to when back button is selected.
    local list = {}  -- table of list items to return

    local isInFolder = not IsEmpty(folderName)
    local hasSearch = not IsEmpty(searchText)
    local hasFilter = not (IsEmpty(filter) or filter == "all")

    if (not isInFolder and not hasSearch and not hasFilter) then
        -- at the root level and no filtering, show all folders and items
        InsertRootFolders(list)
        InsertAll(list)
    elseif (isInFolder and not hasSearch and not hasFilter) then
        -- filter by Genre
        back = "" -- Back to the root folder
        FilterByGenre(list, folderName, false)
    elseif (not isInFolder and hasSearch and not hasFilter) then
        -- search text only
        FilterText(list, searchText)
    elseif (not isInFolder and not hasSearch and hasFilter) then
        -- filter only
        FilterByPeriod(list, filter)
    elseif (isInFolder and not hasSearch and hasFilter) then
        back = "" -- Back to the root folder
        FilterGenrePeriod(list, folderName, filter)
    elseif (isInFolder and hasSearch and not hasFilter) then
        back = "" -- Back to the root folder
        FilterGenreText(list, folderName, searchText)
    elseif (not isInFolder and hasSearch and hasFilter) then
        FilterPeriodText(list, filter, searchText)
    elseif (isInFolder and hasSearch and hasFilter) then
        back = "" -- Back to the root folder
        FilterGenrePeriodText(list, folderName, filter, searchText)
    end

    return list, back
end
```

The GetListAll function is provided for the "CUSTOM\_SELECT All" action to call when it needs the list of items available to select. The "CUSTOM\_SELECT All" action example combines all the features except the asynchronous examples into a single action.


**3. Handle the Action Command**

All driver commands are handled in the ExecuteCommand function. To illustrate the handling of this action, we can update the "CUSTOM\_SELECT Action Search Result" property in the driver with the result of the action. Simply replace that code with whatever action is required for your driver’s needs.

_Handling of **CUSTOM\_SELECT\_All** action in Execute Command_

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
ACTION: The value of this item is the *[Action Command][3]* of the action executed. In this case it will be CUSTOM\_SELECT\_All.  
"Select List Item”: This parameter is the *[Parameter Name][4]* as defined in the xml. Its value is the value returned from the action.






[1]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options
[2]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-custom-select-function
[3]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options
[4]:	https://snap-one.github.io/docs-driverworks-fundamentals/#custom-select-implementation-favored-and-blocked-av-connection-classes-action-configuration-options