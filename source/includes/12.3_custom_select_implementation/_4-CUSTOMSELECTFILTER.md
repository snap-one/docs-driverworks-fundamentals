## CUSTOM\_SELECT Filter


This example adds a filter drop down to the dialog with the filter list provided by the xml. Note that this is a static filter list. Filter list content is defined within the `<search></search>` tags of the action’s XML.


**1. Define the Action**

The following xml defines the action for CUSTOM\_SELECT Filter.

```xml
<action>
    <name>CUSTOM_SELECT Filter</name>
    <command>CUSTOM_SELECT_Filter</command>
    <sort_order>3</sort_order>
    <params>
        <param>
            <name>Select Channel</name>
            <type>CUSTOM_SELECT:GetFilterList</type>
            <search>
                <filters>
                    <filter name="all">All</filter>
                    <filter name="country">Country</filter>
                    <filter name="jazz">Jazz</filter>
                    <filter name="oldies">Oldies</filter>
                    <filter name="pop">Pop</filter>
                    <filter name="rock">Rock</filter>
                </filters>
            </search>
        </param>
    </params>
</action>
```

In the example above:
The [Action Command][1] = CUSTOM\_SELECT\_Filter.  
The [Custom Select Function][2] = GetFilterList.

 **2. Implement the Custom Select Function**

_Implementation of GetFilterList()_

```lua
function GetFilterList(folderName, callback, searchText, filter)
    local list = {} -- table of list items to return
    local hasSearch = not IsEmpty(searchText)
    local hasFilter = not (IsEmpty(filter) or filter == "all")

    if not hasSearch and not hasFilter then
        InsertAll(list)
    elseif hasSearch and not hasFilter then
        FilterText(list, searchText)
    elseif not hasSearch and hasFilter then
        FilterByGenre(list, filter, false)
    else
        -- has both searchText and filter
        FilterGenreText(list, filter, searchText)
    end

    return (list)
end
```

The GetFilterList function is provided for the "CUSTOM\_SELECT Filter" action to call when it needs the list of items available to select. The "CUSTOM\_SELECT Filter" action example is a flat list with both text filtering and/or matches the music genre contained in the filter parameter. The filter parameter will be one of the \<filter\> items driver.xml file. If both the searchText and filter parameters are empty, the full list is returned, Otherwise, only items that contain the search text and match the genre will be returned.


**3. Handle the Action Command**

All driver commands are handled in the ExecuteCommand function. To illustrate the handling of this action, we update the "CUSTOM\_SELECT Action Search Result" property in the driver with the result of the action. Simply replace that code with whatever action is required for your driver’s needs.

_Handling of **CUSTOM\_SELECT\_Filter** action in Execute Command_

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
ACTION: The value of this item is the *[Action Command][3]* of the action executed. In this case it will be CUSTOM\_SELECT\_Filter.  
"Select Channel”: This parameter is the *[Parameter Name][4]* as defined in the xml. Its value is the value returned from the action.








[1]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options
[2]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-custom-select-function
[3]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options
[4]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options