
Commands come from the Device Proxy. For example, a AV Switch might have the following section in the XML of the .c4z file:

```xml
<commands>
 <command>
   <name>MAIN_ZONE_ON</name>
   <description>MAIN_ZONE_ON</description>
 </command>
 <command>
   <name>MAIN_ZONE_OFF</name>
   <description>MAIN_ZONE_OFF</description>
 </command>
 <command>
   <name>ZONE2_ON</name>
   <description>ZONE2_ON</description>
 </command>
 <command>
   <name>ZONE2_OFF</name>
   <description>ZONE2_OFF</description>
 </command>
 <command>
   <name>ZONE3_ON</name>
   <description>ZONE3_ON</description>
 </command>
 <command>
   <name>ZONE3_OFF</name>
   <description>ZONE3_OFF</description>
 </command>
 <command>
   <name>ZONE4_ON</name>
   <description>ZONE4_ON</description>
 </command>
 <command>
   <name>ZONE4_OFF</name>
   <description>ZONE4_OFF</description>
 </command>
</command>
```


## Supported Command Parameter Types


### Custom Select

```xml
<command>
		<name>Select Channel</name>
		<description>Select Channel PARAM1</description>
		<sort_order>9</sort_order>
		<params>
			<param>
				<name>Channel</name>
				<type>CUSTOM_SELECT:SelectChannelParamSelect</type>
			</param>
		</params>
</command>
```

The ability of a Proxy driver to deliver a browse-able list of related elements can be supported through the use of the [CUSTOM\_SELECT property][1]. 

These lists could represent stations, channels or actual media. The elements delivered in the list can then be used as parameter in a command.

In order for this property to work, a command needs to be created in the \<devicedata\>\<config\> section of the c4z driver.xml file. This command must include the CUSTOM\_SELECT parameter type. For example, here is a command called SelectChannelParamSelect.

In the SelectChannel example above, SelectChannel is the command name that is sent to the ExecuteCommand function in the driver. The param type CUSTOM\_SELECT allows for the specification of a global .lua function that will be invoked whenever ComposerPro wants to get data. In the example to the right, it is the function SelectChannelParamSelect.


### Device Selector

```xml
<command>
		<name>DEVICE_SELECTOR Parameter Example</name>
		<description>NAME DEVICE_SELECTOR Value: PARAM1</description>
		<sort_order>6</sort_order>
		<params>
			<param>
				<name>DEVICE_SELECTOR</name>
				<type>DEVICE_SELECTOR</type>
				<items>
					<item>pool.c4i</item>
					<item>camera.c4i</item>
					<item>blind.c4i</item>
					<item>camera.c4i</item>
					<item>controller.c4i</item>
					<item>doorstation.c4i</item>
					<item>light_v2.c4i</item>
					<item>lock.c4i</item>
					<item>media.c4i</item>
					<item>tv.c4i</item>
					<item>uidevice.c4i</item>
					<item>thermostatV2.c4i</item>
					<item>driver_properties.c4z</item>
				</items>
				<multiselect>true</multiselect>
			</param>
		</params>
</command>
```

The Device Selector command parameter type supports the [Device Selector property][2]. This is  a property that can be added to a DriverWorks driver. Device Selector allows you (as the driver developer) to display a list of devices that may be associated with the driver you have developed. These devices can then be selected from within composer. 

In order for this property to work, a command needs to be created in the \<devicedata\>\<config\> section of the c4z driver.xml file. This command must include the DEVICE\_SELECTOR\_ parameter type.


### Dynamic List

```xml
<command>
		<name>DYNAMIC_LIST Parameter Example</name>
		<description>DYNAMIC_LIST Value: PARAM1</description>
		<sort_order>4</sort_order>
		<params>
			<param>
				<name>DYNAMIC_LIST</name>
				<type>DYNAMIC_LIST</type>
			</param>
		</params>
</command>
```

The Dynamic List command parameter type supports the ability to use the [Dynamic List Property][3]. This property provides ability to include driver-based, dynamically updated lists in ComposerPro’s Advanced Properties screen. 

In order for this property to work, a command needs to be created in the \<devicedata\>\<config\> section of the c4z driver.xml file. This command must include the DYNAMIC\_LIST\_ parameter type.

Note: The driver must implement a function called [GetCommandParamList][4] that returns a table with the list for the specified parameter.

### List

```xml
<command>
		<name>LIST Parameter Example</name>
		<description>NAME LIST Value: PARAM1</description>
		<sort_order>3</sort_order>
		<params>
			<param>
				<name>LIST</name>
				<type>LIST</type>
				<items>
					<item>Item 1</item>
					<item>Item 2</item>
					<item>Item 3</item>
					<item>Item 4</item>
				</items>
			</param>
		</params>
</command>
```


The List command parameter type supports the ability to provide a non-dynamic list of selectable objects within the [Properties][5] tab in ComposerPro. 

In order for this property to work, a command needs to be created in the \<devicedata\>\<config\> section of the c4z driver.xml file. This command must include the LIST parameter type.


### Ranged Float

```xml
<command>
		<name>RANGED_FLOAT Parameter Example</name>
		<description>NAME RANGED_FLOAT Value: PARAM1</description>
		<sort_order>1</sort_order>
		<params>
			<param>
				<name>RANGED_FLOAT</name>
				<type>RANGED_FLOAT</type>
				<minimum>50</minimum>
				<maximum>60</maximum>
			</param>
		</params>
</command>
```

The Ranged Float command parameter type supports the ability to provide a range of selectable floating-point or non-integer numbers within the [Properties][6] tab in ComposerPro. 

In order for this property to work, a command needs to be created in the \<devicedata\>\<config\> section of the c4z driver.xml file. This command must include the RANGED\_FLOAT\_ parameter type.


### Ranged Integer

```xml
<command>
		<name>RANGED_INTEGER Parameter Example</name>
		<description>NAME RANGED_INTEGER Value: PARAM1</description>
		<sort_order>0</sort_order>
		<params>
			<param>
				<name>RANGED_INTEGER</name>
				<type>RANGED_INTEGER</type>
				<minimum>79</minimum>
				<maximum>120</maximum>
			</param>
		</params>
</command>
```

The Ranged Integer command parameter type supports the ability to provide a range of selectable integers within the [Properties][7] tab in ComposerPro. 

In order for this property to work, a command needs to be created in the \<devicedata\>\<config\> section of the c4z driver.xml file. This command must include the RANGED\_INTEGER\_ parameter type


### String

```xml
<command>
		<name>STRING Parameter Example</name>
		<description>NAME STRING Value: PARAM1</description>
		<sort_order>2</sort_order>
		<params>
			<param>
				<name>STRING</name>
				<type>STRING</type>
			</param>
		</params>
</command>
```

The STRING command parameter type supports the ability to provide a read only string within the [Properties][8] tab in ComposerPro. 

In order for this property to work, a command needs to be created in the \<devicedata\>\<config\> section of the c4z driver.xml file. This command must include the STRING\_ parameter type


### Variable Selector

```xml
<command>
		<name>VARIABLE_SELECTOR  Parameter Example</name>
		<description>Param Values NAME (boolean: PARAM1, P2: PARAM2, P3: PARAM3, P4: PARAM4, P5: PARAM5, P6: PARAM6, P7: PARAM7, P8: PARAM8)</description>
		<sort_order>7</sort_order>
		<params>
			<param>
				<name>boolean VARIABLE_SELECTOR</name>
				<type>VARIABLE_SELECTOR</type>
				<variabletype>boolean</variabletype>
			</param>
			<param>
				<name>string VARIABLE_SELECTOR</name>
				<type>VARIABLE_SELECTOR</type>
				<variabletype>string</variabletype>
			</param>
			<param>
				<name>number VARIABLE_SELECTOR</name>
				<type>VARIABLE_SELECTOR</type>
				<variabletype>number</variabletype>
			</param>
			<param>
				<name>float VARIABLE_SELECTOR</name>
				<type>VARIABLE_SELECTOR</type>
				<variabletype>float</variabletype>
			</param>
			<param>
				<name>device VARIABLE_SELECTOR</name>
				<type>VARIABLE_SELECTOR</type>
				<variabletype>device</variabletype>
			</param>
			<param>
				<name>media VARIABLE_SELECTOR</name>
				<type>VARIABLE_SELECTOR</type>
				<variabletype>media</variabletype>
			</param>
			<param>
				<name>user VARIABLE_SELECTOR</name>
				<type>VARIABLE_SELECTOR</type>
				<variabletype>user</variabletype>
			</param>
			<param>
				<name>all VARIABLE_SELECTOR</name>
				<type>VARIABLE_SELECTOR</type>
				<variabletype>all</variabletype>
			</param>
		</params>
</command>
```

The Variable Selector command parameter type supports the ability to choose a variable type within the [Properties][9] tab in ComposerPro. Supported Variable Types include: 

- boolean
- string
- number
- float
- device
- media
- user
- room

### Variable Selector Value

```xml
<command>
		<name>VARIABLE_SELECTOR_VALUE Parameter Example</name>
		<description>Param Values NAME(P1: PARAM1, P2: PARAM2, P3: PARAM3, P4: PARAM4, P5: PARAM5, P6: PARAM6)</description>
		<sort_order>8</sort_order>
		<params>
			<param>
				<name>boolean VARIABLE_SELECTOR_VALUE</name>
				<type>VARIABLE_SELECTOR_VALUE</type>
				<variabletype>boolean</variabletype>
			</param>
			<param>
				<name>string VARIABLE_SELECTOR_VALUE</name>
				<type>VARIABLE_SELECTOR_VALUE</type>
				<variabletype>string</variabletype>
			</param>
			<param>
				<name>number VARIABLE_SELECTOR_VALUE</name>
				<type>VARIABLE_SELECTOR_VALUE</type>
				<variabletype>number</variabletype>
			</param>
			<param>
				<name>float VARIABLE_SELECTOR_VALUE</name>
				<type>VARIABLE_SELECTOR_VALUE</type>
				<variabletype>float</variabletype>
			</param>
			<param>
				<name>device VARIABLE_SELECTOR_VALUE</name>
				<type>VARIABLE_SELECTOR_VALUE</type>
				<variabletype>device</variabletype>
			</param>
			<param>
				<name>media VARIABLE_SELECTOR_VALUE</name>
				<type>VARIABLE_SELECTOR_VALUE</type>
				<variabletype>media</variabletype>
			</param>
		</params>
	</command>
```


## GetCommandParamList

This function is required in order for a driver to use the DYNAMIC\_LIST type parameters within a \<command\>.  

### Signature

`GetCommandParamList ()`


| Parameter | Description |
| --- | --- |
| str | commandName | 
| str | paramName |


### Returns

Returns a table with the list for the specified parameter.  


### Example

```lua
function GetCommandParamList(commandName, paramName)
    local tList = {}
 
    if (commandName == "Turn On") then
        if (paramName == "Zone") then
            tList = {"Main", "Master", "Upstairs", "Downstairs", "Patio"}
        elseif (paramName == "Level") then
            tList = {"Low", "Medium", "High"}
        end
    elseif (commandName == "Toggle" and paramName == "Zone") then
        tList = {"Main", "Patio"}
    end
     
    return (tList)
end
```


## Dynamic List type for Commands

Support for DYNAMIC\_LIST type parameters within a \<command\>. The driver must implement a function called GetCommandParamList that returns a table with the list for the specified parameter. 

_This feature was introduced in O.S. Release 3.3.0_


### Example

```xml
<command>
    <name>Turn On</name>
    <description>Turn on PARAM1 to PARAM2</description>
    <params>
        <param>
            <name>Zone</name>
            <type>DYNAMIC_LIST</type>
        </param>
        <param>
            <name>Level</name>
            <type>DYNAMIC_LIST</type>
        </param>
    </params>
</command>
<command>
    <name>Toggle</name>
    <description>Toggle PARAM1</description>
    <params>
        <param>
            <name>Zone</name>
            <type>DYNAMIC_LIST</type>
        </param>
    </params>
</command>
```


## Sorting Commands in ComposerPro


The ability to order the way driver commands are displayed in the Programming area of ComposerPro is possible with the use of a sort order driver XML tag:

```xml
            <command>
                <name>New Command</name>
                <description>New Description</description>
                <sort_order>0</sort_order>
            </command>
            <command>
                <name>New Command 1</name>  
                <description>Command Description 1</description>
                <sort_order>1</sort_order>
            </command>
            <command>
                <name>New Command 2</name>
                <description>Command Description 2</description>
                <sort_order>2</sort_order>
            </command>
            <command>
                <name>New Command 3</name>
                <description>Command Description 3</description>
                <sort_order>3</sort_order>
            </command>
            <command>
                <name>New Command 4</name>
                <description>Command Description 4</description>
                <sort_order>4</sort_order>
            </command>
            <command>
                <name>New Command 5</name>
                <description>Command Description 10</description>
                <sort_order>5</sort_order>
            </command>
```

`<sort_order></sort_order>` 

The tag accepts a zero based list and will display the commands in the order of the number included in each commands’ `<sort_order></sort_order>`tag.

For example, in the XML code to the right there are five device specific commands. They will display in ComposerPro Programming as:

- New Command
- New Command 1
- New Command 2
- New Command 3
- New Command 4
- New Command 5

[1]:	https://control4.github.io/docs-driverworks-fundamentals/#using-the-device-selector-property
[2]:	https://control4.github.io/docs-driverworks-fundamentals/#using-the-device-selector-property
[3]:	https://control4.github.io/docs-driverworks-fundamentals/#dynamic-list-properties
[4]:	https://control4.github.io/docs-driverworks-draft/#3-3-0-new-proxy-commands
[5]:	https://control4.github.io/docs-driverworks-fundamentals/#properties
[6]:	https://control4.github.io/docs-driverworks-fundamentals/#properties
[7]:	https://control4.github.io/docs-driverworks-fundamentals/#properties
[8]:	https://control4.github.io/docs-driverworks-fundamentals/#properties
[9]:	https://control4.github.io/docs-driverworks-fundamentals/#properties