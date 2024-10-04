## CUSTOM\_SELECT Async All


This example supports an Asynchronous call for the list of data found in the `g_tMusicGenre`. While our example uses static data, CUSTOM\_SELECT Async All is useful for querying data from an online resource such as a music service.

**1. Define the Action**

The following xml defines the action for CUSTOM\_SELECT Async All.

```xml
~<action>~
    <name>CUSTOM_SELECT Async All</name>
    <command>CUSTOM_SELECT_Async</command>
    <sort_order>5</sort_order>
    <params>
        <param>
            <name>Select List Item</name>
            <type>CUSTOM_SELECT:GetAsyncListAll</type>
        </param>
    </params>
</action>
```

In the example above:
The [Action Command ][1]= CUSTOM\_SELECT\_Async.  
The [Custom Select Function][2] = GetAsyncListAll.

 **2. Implement the Custom Select Function**

_Implementation of GetAsyncListAll()_

```lua
function GetAsyncListAll(folderName, callback)
    local ticketId = C4:urlGet("http://www.example.com")

    -- Add an entry to g_tickets to store the callback function and a variable myAction
    -- so that the ReceivedAsync call will know what function issued the C4:urlGet command.
    g_tickets[ticketId] = { callbackFunction = callback, myAction = "GetAsyncListAll" }

    -- Do not return anything here, the ReceivedAsync callback function will be called
    -- after the C4:urlGet function completes
end
```

GetAsyncListAll function is provided for the "CUSTOM\_SELECT Async All" action to call when it needs the list of items available to select. The "CUSTOM\_SELECT Async All" action example shows how to handle the list data if an asynchronous call is needed to return the list such as a C4:urlGet command. In this example we will be returning the full list. There are no filtering capabilities in this example.


**3. Handle the Action Command**

All driver commands are handled in the ExecuteCommand function. To illustrate the handling of this action, we can update the "CUSTOM\_SELECT Action Search Result" property in the driver with the result of the action. Simply replace that code with whatever action is required for your driver’s needs.

_Handling of **CUSTOM\_SELECT\_Async** action in Execute Command_

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
ACTION: The value of this item is the *[Action Command][3]* of the action executed. In this case it will be CUSTOM\_SELECT\_Async.  
"Select List Item”: This parameter is the *[Parameter Name][4]* as defined in the xml. Its value is the value returned from the action.

### Additional Async Examples

The sample driver includes two additional examples that can be used to demonstrate support for asynchronous data requests:

**GetAsyncListCountry**

```lua
function GetAsyncListCountry(folderName, callback)
    local ticketId = C4:urlGet("http://www.example.com")

    -- Add an entry to g_tickets to store the callback function and a variable myAction
    -- so that the ReceivedAsync call will know what function issued the C4:urlGet command.
    g_tickets[ticketId] = { callbackFunction = callback, myAction = "GetAsyncListCountry" }

    -- Do not return anything here, the ReceivedAsync callback function will be called
    -- after the C4:urlGet function completes
end
```

GetAsyncListCountry function is provided for the "CUSTOM\_SELECT Async Country" action to call when it needs the list of items available to select. The "CUSTOM\_SELECT Async Country" action example shows how to handle the list data if an asynchronous call is needed to return the list such as a C4:urlGet command. This example returns only country songs. There are no filtering capabilities with this action.

**GetAsyncTimer**

```lua
function GetAsyncListTimer(folderName, callback)
    local timerId = C4:AddTimer(5, "SECONDS")

    -- Add an entry to g_timers to store the callback function.
    g_timers[timerId] = { callbackFunction = callback }

    -- Do not return anything here, the OnTimerExpired function will be called
    -- after the timer expires.
end
```

GetAsyncListTimer function is provided for the "CUSTOM\_SELECT Async Timer action to call. The "CUSTOM\_SELECT Async Timer action example uses a five second timer to demonstrate an asynchronous call. After five seconds expire, it returns a list of all of the entries in the `g_tMusicGenre` table.

[1]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options
[2]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-custom-select-function
[3]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options
[4]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-implementation-common-directories-action-configuration-options