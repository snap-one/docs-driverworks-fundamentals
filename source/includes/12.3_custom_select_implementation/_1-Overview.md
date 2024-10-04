
# CUSTOM SELECT Implementation

Note: A sample driver has been delivered to assist with understanding and implementing custom select functionality in LUA drivers. The driver is named sample\_custom\_select.c4z. It can be found in the DriverWorks Github repository in the Sample Drivers folder: 

[https://github.com/snap-one/docs-driverworks/tree/master/sample\_drivers][1]

The examples used in this documentation come from this driver. The driver can be loaded into a project and all of the supported CUSTOM\_SELECT features can be exercised. Use of the sample driver is intended for O.S. 4.0.0 or later. However, it can be used in prior operating systems with the understanding that an earlier O.S. will result in inconsistent performance specifically surrounding the CUSTOM\_SELECT Search and Filter types.

To make full use of the documentation, it is recommended that the driver be opened in your preferred development environment as several of the driver’s files will be reviewed and discussed in the examples included here.

The CUSTOM\_SELECT type is available for both actions and commands for a driver. You’ll note identical Actions and Commands are defined in the sample XML and LUA code found in the sample\_custom\_select.c4z driver. While this documentation focuses on Action to demonstrate the CUSTOM\_SELECT functionality, implementation of commands is identical. 

The driver provides a list of selectable items in Composer Pro. Once an item is selected, it is returned to the driver. For Programming in Composer Pro, the driver provides a list of items to select that are used in a command for the driver. This enables actions and programming to have dynamic lists. 

The ability of a Driver to deliver a list of related data-elements is supported through the use of the CUSTOM\_SELECT XML property. The delivery of these lists can be very useful for your driver as they could represent radio stations, TV channels, light levels or URLs. It is also possible to create a “browsing experience” in Composer Pro with folders that contain the list data.  Once these data-elements are delivered, they can then be used as a parameter in ComposerPro programming. 

For example, consider that a driver supports the ability to play audio from a database. The CUSTOM\_SELECT type can be implemented to provide several user experiences in ComposerPro with regard to selecting audio. It can return simple list of audio choices and searching the list is also supported. The audio can also be organized in folders to create a browsable experience. The CUSTOM\_SELECT type also supports the use of asynchronous calls in the case where it is used with an online audio service provider. All of these use cases and their respective implementations are detailed in the documentation that follows.

Currently, the following CUSTOM\_SELECT features are supported:

- **List**: Returns a simple list with no filtering options using minimal required parameters.
- **Search**: Adds a search box to the dialog.
- **Filter**: Adds a filter drop down to the dialog with the list provided by the xml. This is a static filter list.
- **Folder**: Adds a tree view type control. Note that folder and folder traversal is determined by the driver.
- **Async**: Supports an Asynchronous call for the list.

Throughout this documentation we’ll refer to several Composer Pro UI constructs. These are found within the Actions dialog box. They include:

1. List of returned data.
2. Folders which can be use to organize data.
3. Search box that supports string entry for searching data.
4. Filter drop down box that displays the static filters defined in the driver’s XML.
5. OK button which executes the ExecuteCommand Lua function.

<img src="images/csimage100.png"/>


### How Actions work in Your Driver

Before implementing CUSTOM\_SELECT action functionality in a .lua driver, it is necessary to understand how driver Actions work. Actions are defined in your driver’s \<actions\> section of the drivers.xml file. They are displayed under the Actions tab in the Properties area of Composer Pro. 

When an action is selected and executed in Composer Pro, a `LUA_ACTION` command is sent to the driver with any parameters the action may have. These commands are handled in the driver's ExecuteCommand function. You can see this code in the sample driver's driver.lua file.

The sample driver’s implementation of this is all handled within the ExecuteCommand function. The `LUA_ACTION` command is where all actions are handled. With programming, each command is handled individually.

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
            else
                print ("Action not found: " .. action)
            end
        end
    elseif sCommand == "CUSTOM_SELECT List" then
        selectedItem = tParams["Select List Item"]
        C4:UpdateProperty("CUSTOM_SELECT Command List Result", tostring(selectedItem))
    elseif sCommand == "CUSTOM_SELECT Search" then
        selectedItem = tParams["Select List Item"]
        C4:UpdateProperty("CUSTOM_SELECT Command Search Result", tostring(selectedItem))
    elseif sCommand == "CUSTOM_SELECT Filter" then
        selectedItem = tParams["Select Channel"]
        C4:UpdateProperty("CUSTOM_SELECT Command Filter Result", tostring(selectedItem))
    elseif sCommand == "CUSTOM_SELECT Folder" then
        selectedItem = tParams["Select List Item"]
        C4:UpdateProperty("CUSTOM_SELECT Command Folder Result", tostring(selectedItem))
    elseif sCommand == "CUSTOM_SELECT Async All" then
        selectedItem = tParams["Select List Item"]
        C4:UpdateProperty("CUSTOM_SELECT Command Async Result", tostring(selectedItem))
    elseif sCommand == "CUSTOM_SELECT Async Country" then
        selectedItem = tParams["Select List Item"]
        C4:UpdateProperty("CUSTOM_SELECT Command Async2 Result", tostring(selectedItem))
    elseif sCommand == "CUSTOM_SELECT All" then
        selectedItem = tParams["Select List Item"]
        C4:UpdateProperty("CUSTOM_SELECT Command All Result", tostring(selectedItem))
    else
        print ("Error: unsuppoted command: ".. sCommand)
    end
end
```

The ExecuteCommand takes two parameters. The first parameter contains the command name being sent to the driver. The second parameter is a table of all of the parameters for the command. In the case of the sample driver, if the command is not handled an error message is printed and no action is taken.


### Action Configuration Options

_sample driver action example:_

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


`<name>`

- Required
- The \<name\> element contains the name of the Action. This name appears in Composer Pro’s list of actions.
- In this documentation this is referred to as the **"Action Name"**.


`<command>`

- Required
- When an action is executed, a key/value pair entry is added to the tParams _(the second parameter to the ExecuteCommand function)_ table with the key equal to `ACTION` and the value equal to the value of the \<command\> element. In the following example below, we can see the Action is the key and command CUSTOM\_SELECT\_filter is the value: `{"ACTION":"CUSTOM_SELECT_Filter"}`.
- In this documentation, this is referred to as the **“Action Command”**


`<sort_order>`

- Optional
- Numeric value that specifies the order the action appears in the list of Actions in Composer Pro.


`<param><name>`

- Required
- Name of the parameter that will be displayed in the parameter list in Composer Pro.
- In this documentation, this is referred to as the **"Parameter Name"**


 `<param><type>`

- Required
- The type of the parameter. The type for custom select action parameter is: `CUSTOM_SELECT:function_name`. The function\_name is the name of the function that must be implemented in the driver and will return the desired list and shown in Composer Pro. In the example above, the function name is `GetFilterList`. The sample driver's custom select functions are found in the custom\_select.lua file.
- In this documentation, this is referred to as the **“Custom Select Function”**.


`<param><search>`

- Optional
- Provides a search text box for filtering the list items. Use an empty tag to show the text box without the filter combo box control.


 `<search><filters>`

- Optional
- A filter combo box will be added to the Select dialog. The combo box contains the list of strings defined by the filter elements in the xml. How the filters are used is determined by the driver.


 `<filter>`

- Requires at least two \<filter\> elements for filtering.
- There needs to be one filter element for each item in the list. The name attribute value is used when selecting the item from the list. The element value is the text that is displayed in the combo box.


### Implementation Steps
The list below represent the three fundamental steps required to implement CUSTOM\_SELECT type. The fourth step is only required when supporting Asynchronous type actions. These steps will be referred to throughout the documentation.

1. Define the Action in the drivers.xml file.
2. Implement the action's **Custom Select Function** defined in the \<param\>\<type\> element. This function implementation in the sample driver is found in the custom\_select.lua file.
3. Handle the **“Action Command”** within the ExecuteCommand function.
4. If there are any Asynchronous type actions, handle the results in the ReceivedAsync function. This is found in the driver.lua in the sample driver.


### Custom Select Function
The Custom Select Function's purpose is to provide the data for the *Select* dialog in Composer Pro. This dialog has numerous capabilities that will be described below.


**_Parameters_**

The function parameters are optional depending on the desired functionality of the *Select* dialog. There are a maximum of four parameters. These are defined below. In the sample driver used with this documentation, when no parameters are passed, the function returns a list of all the items. 


_Parameter 1 - string_ (folder)  
This parameter is used for the tree view (folder) functionality and will contain the name of a selected folder. If the parameter is empty then that indicates Composer wants the list at the root level. Otherwise, it will contain a folder name and the function will return the list of items within that folder.

_Parameter 2 - function_ (callback)  
This parameter is typically used in asynchronous actions. However, it can be used in place of using return values. This function will be described below.

_Parameter 3 - string_ (search)  
This parameter contains the text entered in the search box of the Select dialog. This is handled more like a filter rather than a search. The Custom Select Function should return just those items that contain the text from this parameter.

_Parameter 4 - string_ (filter)  
This parameter contains the text value of the selected item in the filter combo box in the search control. It is up to the driver developer to determine how to use this parameter. For example, it may contain a list of music genres, in which case the Custom Select Function should return a list of albums or songs that match that genre.

_**Callback Function (parameter 2)**_  
A callback function is provided in those cases where the list data is not available when the function is called as will be the case with asynchronous actions. The driver must save this function for use when the driver is ready to return the data.

_**Return Values/Parameters for the Callback Function**_  
There are two ways to return the results of the Custom Select Function: function return values or execute the callback function provided by parameter 2. In both cases there are two parameters or return items: the list of items and the name of the folder for use when the back button is selected. Executing the callback function is required for asynchronous actions since the data is not available at the time the Custom Select Function is called.

_Return/Parameter 1 - table_ (list of items)  
Table of list items. The format is described below.

_Return/Parameter 2 - string_ (back)  
The name of the folder to open when the back button is pressed. If empty, then it will return back to the root folder level.

_**Return List Format**_

Each entry in the table has three key, value pair items. Two are required and one is optional.
text
- string type
- Required
- Contains the text that is displayed.

value
- string type
	- Required
	- Value returned when the item is selected.

folder
- type boolean
	- Optional
	- default = false
	- Specifies if the item is a folder item. If = true, the item will be displayed as a folder. When a folder is selected, The Custom Select Function is called with the first parameter equal to the value in the table entry.

_sample list creation_

```lua
-- create table of list items
list = {}
-- add some folders
table.insert(list, { text = "Country", value = "country", folder = true })
table.insert(list, { text = "Jazz", value = "jazz", folder = true })
table.insert(list, { text = "Oldies", value = "oldies", folder = true })
table.insert(list, { text = "Pop", value = "pop", folder = true })
table.insert(list, { text = "Rock", value = "rock", folder = true })
-- add some list items
table.insert(list, { text = '50s Rock and Roll', value = 'rock_50' })
table.insert(list, { text = '60s Oldies', value = 'oldies_60' })
table.insert(list, { text = '60s Pop', value = 'pop_60' })
table.insert(list, { text = '70s Pop', value = 'pop_70' })
```


### Understanding Action Workflow
If a driver has any Actions defined, there will be an Actions tab On the driver's System Design page. After selecting the Action tab, a list of actions will be displayed. This list comes from the list of actions in the drivers.xml file. 

Upon selecting an action, the `Action Parameter List` dialog will appear. Any actions that are CUSTOM\_SELECT type actions will have a lookup button to the left of the parameter. Upon selecting this button,  the function declared in the CUSTOM\_SELECT type field is called. This function handles the creation of the list that is displayed in the `Select` dialog.
 _Note: How this function interacts with Composer Pro Actions will be described in each of the CUSTOM\_SELECT feature documentation sections below._ 

After selecting an item in the list and clicking Ok, you will be returned back to the `Action Parameter List` with the selected value in the action value field. 

Clicking Ok on the `Action Parameter List` dialog will call the action command defined in the actions xml. This command will receive a table of results from the action where the driver handles those results.

The following represents a LUA Driver/Composer Pro workflow which begins with the user selecting the Actions tab in Composer Pro and ends with the execution of the ExecuteCommand function where the action is handled:

- User selects Actions Tab on the Driver's System Design page.
	- The Action List is Displayed.
	- User selects the action from the list.
		- The Action Parameter List dialog is displayed with the list of parameter(s).
		- User selects the Lookup Button for the action.
			-The action's Custom Select Function is called, which returns a list or saves off the callback function for asynchronous type actions.
			- The list is displayed in the Select dialog either from the return values or when the asynchronous process completes and returns the list.
			- User finds the desired list item and selects the item.
			- User Clicks the Ok button.
		- Back to Action Parameter List where the action parameter is set to the selected item's value
		- User Clicks the Ok button.
	 - The Action’s Command is sent to the driver's ExecuteCommand function where the result of the action is handled.


**Data used for sample\_custom\_select.c4z driver**

The example driver’s driver.lua file has a global table that contains all of the data that is used when exercising the driver and found in this documentation. The table uses music genres and types for example purposes:

```lua
g_tMusicGenre = {}

-- Initialize functions
function OnDriverInit(initType)
    table.insert(g_tMusicGenre, { text = '50s Rock and Roll', value = 'rock_50', genre = "rock", period = "50s" })
    table.insert(g_tMusicGenre, { text = '60s Oldies', value = 'oldies_60', genre = "oldies", period = "60s" })
    table.insert(g_tMusicGenre, { text = '60s Pop', value = 'pop_60', genre = "pop", period = "60s" })
    table.insert(g_tMusicGenre, { text = '70s Pop', value = 'pop_70', genre = "pop", period = "70s" })
    table.insert(g_tMusicGenre, { text = '80s Pop', value = 'pop_80', genre = "pop", period = "80s" })
    table.insert(g_tMusicGenre, { text = 'Classic Country', value = 'country_classic', genre = "country", period = "" })
    table.insert(g_tMusicGenre, { text = 'Classic Rock', value = 'rock_classic', genre = "rock", period = "" })
    table.insert(g_tMusicGenre, { text = 'Contermporary Jazz', value = 'jazz_contermporary', genre = "jazz", period = "" })
    table.insert(g_tMusicGenre, { text = 'Cool Jazz', value = 'jazz_cool', genre = "jazz", period = "" })
    table.insert(g_tMusicGenre, { text = 'Country 2K', value = 'country_2k', genre = "country", period = "2k" })
    table.insert(g_tMusicGenre, { text = 'Country Rock', value = 'country_rock', genre = "rock", period = "" })
    table.insert(g_tMusicGenre, { text = 'Golden Oldies', value = 'oldies_golden', genre = "oldies", period = "" })
    table.insert(g_tMusicGenre, { text = 'Hard Rock', value = 'rock_hard', genre = "rock", period = "" })
    table.insert(g_tMusicGenre, { text = 'Jazz Piano', value = 'jazz_piano', genre = "jazz", period = "" })
    table.insert(g_tMusicGenre, { text = 'Motown', value = 'oldies_motown', genre = "oldies", period = "" })
    table.insert(g_tMusicGenre, { text = 'Pop Love Songs', value = 'pop_lovesongs', genre = "pop", period = "" })
    table.insert(g_tMusicGenre, { text = 'Pop Rock', value = 'pop_rock', genre = "pop", period = "" })
    table.insert(g_tMusicGenre, { text = 'Rock Hits', value = 'rock_hits', genre = "rock", period = "" })
    table.insert(g_tMusicGenre, { text = 'Smooth Jazz', value = 'jazz_smooth', genre = "jazz", period = "" })
    table.insert(g_tMusicGenre, { text = 'Soft Rock', value = 'rock_soft', genre = "rock", period = "" })
    table.insert(g_tMusicGenre, { text = 'The 80s', value = 'country_80', genre = "country", period = "80s" })
    table.insert(g_tMusicGenre, { text = 'The 90s', value = 'country_90', genre = "country", period = "90s" })
    table.insert(g_tMusicGenre, { text = 'Today\'s Country', value = 'country_today', genre = "country", period = "" })
    table.insert(g_tMusicGenre, { text = 'Today\'s Hits', value = 'pop_todays_hits', genre = "pop", period = "" })
end
```

The documentation that follows contains information on implementing CUSTOM\SELECT features including:

**[List][2]**

**[Search][3]**

**[Filter][4]**

**[Folder][5]**

**[Asynch][6]**

All of the supporting code for these features are provided in the sample\_custom\_select.c4z driver.

[1]:	https://github.com/snap-one/docs-driverworks/tree/master/sample_drivers
[2]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-property-implementation-common-directories-custom_select-list
[3]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-property-implementation-common-directories-custom_select-search
[4]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-property-implementation-common-directories-custom_select-filter
[5]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-property-implementation-common-directories-custom_select-folder
[6]:	https://legendary-disco-58bce7a4.pages.github.io/#custom-select-property-implementation-common-directories-custom_select-async