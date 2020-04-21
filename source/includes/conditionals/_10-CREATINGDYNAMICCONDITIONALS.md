## Creating Dynamic Driver Conditionals

All of the preceding conditional content has referred to statically created conditionals defined within the driver’s XML code. While the types and functionality of Dynamic Conditionals are identical to the static variety, they differ in that they are created dynamically by the driver. This is useful in instances where a driver conditionals are based off of the drivers properties or based off of the configuration of the device the driver is connected to. 

For example, consider a Pool Controller where one model of the controller supports spa control and another model does not. One driver could be written for both models containing a `has_spa` property. If that property is selected in ComposerPro or the driver can query spa support, a conditional could be dynamically created to support spa control

When Composer generates the list of conditionals for the driver in Composer Pro programming, it first will create the static conditionals (defined in the xml of the driver). Once that is done it then needs to know about any dynamic conditionals the driver may have. This is handled through the function, GetConditionals. If the driver has defined the GetConditionals function, then that function is called. If the driver does not have any need for dynamic conditionals then there is no need to create the function.

## GetConditionals()
Function that returns a table of information containing elements of conditionals that will be created dynamically in ComposerPro.

### Parameters
`None`

| Returns  | Value |
| --- | --- |
|table |Table containing all the configuration information for the dynamic conditionals of the driver. |

Dynamic conditionals support all the same types that are supported with static conditionals. The example below details all the conditional types previously defined above. The table, tConditionals, includes an example of each of those conditionals and the table elements required for that conditional to be created dynamically in Composer Pro. For example, the XML required to create a static SIMPLE Conditional Type looks like this:

`1.  <conditionals>`
`2.   <conditional>`
`3.     <id>10</id>`
`4.     <name>SIMPLE_LIGHT_ON</name>`
`5.     <type>SIMPLE</type>`
`6.     <condition_statement>The light is on</condition_statement>`
`7.     <description>NAME is On</description>`
`8.   </conditional>`
`9.  </conditionals>`

The same conditional’s required table information to enable it to be created dynamically looks like this:

`tConditionals["10"] = {} `
`tConditionals["10"]["name"] = "SIMPLE_LIGHT_ON"`
`tConditionals["10"]["type"] = "SIMPLE"`
`tConditionals["10"]["condition_statement"] = "The Light is on"`
`tConditionals["10"]["description"] = "NAME is On"`


	function GetConditionals()
	    -- starting conditionals with ID 10
	    local tConditionals = {}
	
	    tConditionals["10"] = {} 
	    tConditionals["10"]["name"] = "SIMPLE_LIGHT_ON"
	    tConditionals["10"]["type"] = "SIMPLE"
	    tConditionals["10"]["condition_statement"] = "Light is on"
	    tConditionals["10"]["description"] = "NAME is On"
	
	    tConditionals["11"] = {} 
	    tConditionals["11"]["name"] = "SIMPLE_LIGHT_OFF"
	    tConditionals["11"]["type"] = "SIMPLE"
	    tConditionals["11"]["condition_statement"] = "Light is off"
	    tConditionals["11"]["description"] = "[11] NAME is Off"
	
	    tConditionals["12"] = {} 
	    tConditionals["12"]["name"] = "BOOL_LIGHT"
	    tConditionals["12"]["type"] = "BOOL"
	    tConditionals["12"]["condition_statement"] = "Light is"
	    tConditionals["12"]["description"] = "NAME Light is STRING"
	    tConditionals["12"]["true_text"] = "On"
	    tConditionals["12"]["false_text"] = "Off"
	
	    tConditionals["13"] = {} 
	    tConditionals["13"]["name"] = "BOOL_LIGHT_ON"
	    tConditionals["13"]["type"] = "BOOL"
	    tConditionals["13"]["condition_statement"] = "Light is On"
	    tConditionals["13"]["description"] = "[13] NAME Light is On is STRING"
	    tConditionals["13"]["true_text"] = "True"
	    tConditionals["13"]["false_text"] = "False"
	
	    tConditionals["14"] = {} 
	    tConditionals["14"]["name"] = "NUMBER_LIGHT_LEVEL"
	    tConditionals["14"]["type"] = "NUMBER"
	    tConditionals["14"]["condition_statement"] = "Light Level"
	    tConditionals["14"]["description"] = "NAME is LOGIC INTEGER"
	    tConditionals["14"]["minimum"] = "10"
	    tConditionals["14"]["maximum"] = "150"
	
	    tConditionals["15"] = {} 
	    tConditionals["15"]["name"] = "STRING_LIGHT"
	    tConditionals["15"]["type"] = "STRING"
	    tConditionals["15"]["condition_statement"] = "Light Level"
	    tConditionals["15"]["description"] = "NAME Level is LOGIC STRING"
	
	    tConditionals["16"] = {} 
	    tConditionals["16"]["name"] = "LIST_LIGHT_LEVEL"
	    tConditionals["16"]["type"] = "LIST"
	    tConditionals["16"]["condition_statement"] = "Light Level"
	    tConditionals["16"]["description"] = "NAME is LOGIC STRING"
	    tConditionals["16"]["list_items"] = "10%,20%,30%,40%,50%,60%,70%,80%,90%,100%" 
	
	    tConditionals["17"] = {} 
	    tConditionals["17"]["name"] = "ROOM_SELECTION"
	    tConditionals["17"]["type"] = "ROOM"
	    tConditionals["17"]["condition_statement"] = "Room Selection is"
	    tConditionals["17"]["description"] = "NAME is LOGIC STRING"
	
	    tConditionals["18"] = {} 
	    tConditionals["18"]["name"] = "DEVICE_SELECTION"
	    tConditionals["18"]["type"] = "DEVICE"
	    tConditionals["18"]["condition_statement"] = "Device Selection is"
	    tConditionals["18"]["description"] = "NAME Device Selection LOGIC DEVICE"
	
	    return tConditionals
	end
