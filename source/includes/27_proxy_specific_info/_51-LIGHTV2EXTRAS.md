
## Light Proxy Extras Interface Library

### Overview
Extras are a way for driver developers to expose unique hardware functionality that users can interact with on a UI/Navigator.  This functionality bypasses the need for a proxy to know about the unique features and how to handle them.  A protocol driver notify is used to pass the Extras XML information from the protocol driver to the proxy, where the proxy caches the xml strings for current setup and state information. The UI  uses a UI\_REQUEST as needed to get the information if the UI has just rebooted/started/loaded and then the UI will get updates of these xml strings if it is listening for dataToUI's from the device.  Extras objects are accessible through the "Extras Tab" located on the main Navigator screen for the Proxy.


### Capabilities
The has\_extras capability must be set to true to support the use of Extras. The library will automatically (via dynamic capability) set the has\_extras capability when a first Section is created. Also, when the last section is removed, the has\_extras capability will be disabled. The Extras class does not implement class destructor as it is not supported in Lua 5.1 and Lua JIT. In the case of setting the Extras class instance to nil, the Extras:cleanCapability() method should be called previously.

### Protocol Notify and Proxy Messages
There are two notifies a driver must make to the proxy where this information is forwarded immediately to any UI's listening for dataToUI on the proxy and the information is also cached for future UI's to request: EXTRAS\_SETUP\_CHANGED and EXTRAS\_STATE\_CHANGED.  

Whenever a driver changes values that are reflected in either the setup or state xml, it must make the notify to the proxy with the updated information.  This includes if hardware or a feature is added or removed, a value for one of the objects was updated, and so on.  

It is also advisable for protocol drivers to send this data either on initialization of the driver if the values are not likely to change when hardware comes online, or perhaps when the hardware comes online and the extras commands will be able to succeed.  

In addition, it could be advisable that the state of objects is changed to hidden when hardware and/or disabled features are not available rather than sending new extras\_setup notifies as this may cause a UI to redraw the extras page and reset defaults while a user is interacting with the Extras page on a UI.

### How UI's handle Extras objects

UI's will display objects in the order they are read in the EXTRAS\_SETUP xml.  If an object needs to have a default selection, number or string, the UI looks at the value attribute for the default.  If no value is specified in the EXTRAS\_SETUP the UI will default to a behavior listed below for each object type.

When a user changes a value on a UI, the UI will immediately send the command associated with that object down to a protocol driver as a command.  The UI waits for a response that the protocol driver has handled the command and the protocol driver must send an updated EXTRAS\_STATE notify for the object.  

The EXTRAS\_STATE does not need to include all states of all objects, only the states of the objects it wants to update (or acknowledged) it has received but it could contain more than just one objects updated state.  A driver could also respond back with just the `<extras_state><extra><object id="1"></extra></extras_state>` if it wanted to acknowledge the command succeeded but not update the value.  If a UI that sent the command does not get a response in 10 seconds, it will change the value of the object back to what it was prior to what the user had changed it from.

### Extras Library Use Case Examples

#### Importing the library

To use this library, please move all `*.lua` files to the source directory of your driver, then import `Extras.lua` file in your `driver.lua` file. The `Extras.lua` file includes all other library files needed.

This library depends on a driver template. Please, make sure that the driver template files are included in the path.
**NOTE** Make sure paths are setup correctly

#### How to use the library

**Set Up**

The code to the right is an example how to setup the extras library:

```lua
require "Extas"

function ON_DRIVER_INIT.SetupExtras()
	extras = Extras:new(DEFAULT_PROXY_BINDINGID)
	extras:addSection("ButtonLabel")
	extras:addSection("CheckboxLabel")
	local bt1Table = {
		id = "Button1ID",
		label = "Button 1 Label",
		command = "Button1Command",
		extraparams = {
			Checkbox4ID = "Extr1",
			Checkbox3ID = "Extr2",
			Button2ID = "Extr3"
			}
		}
	extras:addObjects({ExtrasButton:new(bt1Table, "ButtonText1"),
		   ExtrasButton:new({id = "Button3ID", label = "Button 3 Label", command = "Button1Command"}, "ButtonText3"),
		   ExtrasButton:new({id = "Button2ID", label = "Button 2 Label", command = "Button1Command"}, "ButtonText2"),
		   ExtrasButton:new({id = "Button4ID", label = "Button 4 Label", command = "Button1Command"}, "ButtonText4")},
		   "ButtonLabel")
	extras:addObjects({ExtrasCheckbox:new({id = "Checkbox1ID", label = "Checkbox 1 Label", command = "Checkbox1Command"}, "true"),
		   ExtrasCheckbox:new({id = "Checkbox2ID", label = "Checkbox 2 Label", command = "Checkbox1Command"}, "true"),
		   ExtrasCheckbox:new({id = "Checkbox3ID", label = "Checkbox 3 Label", command = "Checkbox1Command"}, "true"),
		   ExtrasCheckbox:new({id = "Checkbox4ID", label = "Checkbox 4 Label", command = "Checkbox1Command"}, "true")},
		   "CheckboxLabel")
	extras:sendSetup(DEFAULT_PROXY_BINDINGID)
end

```
- Section order in the CI will be in the same order section were added in the container class instance.
- Object order in the CI will be in the same order objects were added in the section.
- Remember that object could be added as separated elements or as an array of objects.
- This setup can be called in `DriverLateInit()`. Setting extras in the `DriverInit()` callback is possible but not recommended.


#### Handling Proxy Commands

The next code example shows how to write handlers for proxy commands received when extras change:

```lua
-- Proxy Command Handlers

PRX_CMD.Button1Command(proxyId, tParams)
	C4:SetVariable("Extras_Button1", os.date())
	LogDebug(os.date() .. " " .. extras:getObject(tParams.id).label .. " - Extras Button pressed")
end

function PRX_CMD.Checkbox1Command(proxyId, tParams)
	C4:SetVariable("Extras_Checkbox1", tParams.value)
	LogDebug(os.date() .. " " .. extras:getObject(tParams.id).label .. " - Extras Checkbox switched")
	extras:updateObject(proxyId, tParams.id, tParams.value)
end
```


To handle changes in extras parameters coming from the device side, the  `updateObject` method can be used. This method updates extras object instance and sends proxy notification that extras status has been changed. Also, each `ExtrasObjectBasedClass` class supports the  `ExtrasObjectBasedClass:update(value, hidden)` method that enables extras update from the class instance directly.

It is nice to create a variable for each extras object, to give the partner opportunity to monitor and take action based on extras states. Sample variable definitions are to the right.

```lua
--Variable Examples

C4:AddVariable("Extras_Button1", "", "STRING")
C4:AddVariable("Extras_Checkbox1", "", "BOOL")
```


#### Sqishy

If the Lua Squish is used, the Module code snippet below should be included in the squishy file:

```xml
-- Lua Squish Sample

Module "Extras" "extras/Extras.lua"
Module "ExtrasObject" "extras/ExtrasObject.lua"
Module "ExtrasButton" "extras/ExtrasButton.lua"
Module "ExtrasCheckbox" "extras/ExtrasCheckbox.lua"
Module "ExtrasIcon" "extras/ExtrasIcon.lua"
Module "ExtrasList" "extras/ExtrasList.lua"
Module "ExtrasSwitch" "extras/ExtrasSwitch.lua"
Module "ExtrasNumber" "extras/ExtrasNumber.lua"
Module "ExtrasSlider" "extras/ExtrasSlider.lua"
Module "ExtrasText" "extras/ExtrasText.lua"
Module "ExtrasTextField" "extras/ExtrasTextField.lua"
```

In this case, the library can be used as git submodule.


#### Removing

To remove the object or the section please use following methods:

- `Extras:removeObject(id)`
- `Extras:removeSection(sectionLabel)`

This way, you will avoid lost objects (objects that are in section array but will not be displayed because empty index before).
It is strongly recommended to hide rather than remove objects that will be temporary removed form the CI.

Before destroying an Extras class instance, please use the `Extras:cleanCapability()` method. Also, please pay attention to the Capability section of the documentation.

For additional Information, please see the [DriverWorks Proxy and Protocol Guide][1] regarding all of the functions supported by the Light Proxy.

[1]:	https://expert-adventure-1w2nllv.pages.github.io/#introduction