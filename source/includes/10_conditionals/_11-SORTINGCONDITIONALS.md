## Sorting Conditionals in ComposerPro

Prior to the release of O.S. 3.2.0, driver writers did not have the ability to order the way driver conditionals were displayed in the Programming area of ComposerPro. A new sort order driver element has been delivered that provides the ability to display conditionals in any desired order. Sort order can be applied to static conditionals in the driver.xml with the inclusion of the following xml tag: `<sort_order></sort_order>`. See the lighting code Example 1 from a driver.xml file to the right.


```xml
Example 1

driver.xml
    <conditionals>
      <conditional>
        <id>0</id>
        <name>SIMPLE_LIGHT_ON</name>
        <type>SIMPLE</type>
        <condition_statement>The light is on</condition_statement>
        <description>NAME is On</description>
        <sort_order>0</sort_order>      
      </conditional>
      <conditional>
        <id>1</id>
        <name>SIMPLE_LIGHT_OFF</name>
        <type>SIMPLE</type>
        <condition_statement>The light is off</condition_statement>
        <description>NAME is Off</description>
        <sort_order>2</sort_order>
      </conditional>
      <conditional>
        <id>2</id>
        <name>BOOL_LIGHT</name>
        <type>BOOL</type>
        <condition_statement>Light is</condition_statement>
        <description>NAME Light is STRING</description>
        <true_text>On</true_text>
        <false_text>Off</false_text>
        <sort_order>4</sort_order>
      </conditional>
     <conditional>
        <id>3</id>
        <name>BOOL_LIGHT_ON</name>
        <type>BOOL</type>
        <condition_statement>Light is On</condition_statement>
        <description>NAME Light is On is STRING</description>
        <true_text>True</true_text>
        <false_text>False</false_text>
        <sort_order>6</sort_order>
      </conditional>
      <conditional>
        <id>4</id>
        <name>NUMBER_LIGHT_LEVEL</name>
        <type>NUMBER</type>
        <condition_statement>Light Level</condition_statement>
        <description>NAME is LOGIC INTEGER</description>
        <minimum>10</minimum>
        <maximum>150</maximum>
        <sort_order>8</sort_order>
      </conditional>
      <conditional>
        <id>5</id>
        <name>STRING_LIGHT</name>
        <type>STRING</type>
        <condition_statement>Light Level</condition_statement>
        <description>NAME Level is LOGIC STRING</description>
        <sort_order>10</sort_order>
      </conditional>
      <conditional>
        <id>6</id>
        <name>LIST_LIGHT_LEVEL</name>
        <type>LIST</type>
        <condition_statement>Light Level</condition_statement>
        <description>NAME is LOGIC STRING</description>
        <items>
          <item>10%</item>
          <item>20%</item>
          <item>30%</item>
          <item>40%</item>
          <item>50%</item>
          <item>60%</item>
          <item>70%</item>
          <item>80%</item>
          <item>90%</item>
          <item>100%</item>
        </items>
        <sort_order>12</sort_order>
      </conditional>
    </conditionals>
```

Dynamic Conditionals found in the driver.lua file can also be sorted with the inclusion of the sort order element in the Conditionals table found in the driver.lua file: `["sort_order"] = "1"` See the lighting code Example 2 from a driver.lua file to the right.

```lua
Example 2

function GetConditionals()
 
                local tConditionals = {}
 
                tConditionals["10"] = {}
                tConditionals["10"]["name"] = "SIMPLE_LIGHT_ON"
                tConditionals["10"]["type"] = "SIMPLE"
                tConditionals["10"]["condition_statement"] = "[10-SIMPLE-DYNAMIC] Light is on"
                tConditionals["10"]["description"] = "[10] NAME is On"
                tConditionals["10"]["sort_order"] = "1"
 
                tConditionals["11"] = {}
                tConditionals["11"]["name"] = "SIMPLE_LIGHT_OFF"
                tConditionals["11"]["type"] = "SIMPLE"
                tConditionals["11"]["condition_statement"] = "[11-SIMPLE-DYNAMIC] Light is off"
                tConditionals["11"]["description"] = "[11] NAME is Off"
                tConditionals["11"]["sort_order"] = "3"
 
                tConditionals["12"] = {}
                tConditionals["12"]["name"] = "BOOL_LIGHT"
                tConditionals["12"]["type"] = "BOOL"
                tConditionals["12"]["condition_statement"] = "[12-BOOL-DYNAMIC] Light is"
                tConditionals["12"]["description"] = "[12] NAME Light is STRING"
                tConditionals["12"]["true_text"] = "On"
                tConditionals["12"]["false_text"] = "Off"
                tConditionals["12"]["sort_order"] = "5"
 
                tConditionals["13"] = {}
                tConditionals["13"]["name"] = "BOOL_LIGHT_ON"
                tConditionals["13"]["type"] = "BOOL"
                tConditionals["13"]["condition_statement"] = "[13-BOOL-DYNAMIC] Light is On"
                tConditionals["13"]["description"] = "[13] NAME Light is On is STRING"
                tConditionals["13"]["true_text"] = "True"
                tConditionals["13"]["false_text"] = "False"
                tConditionals["13"]["sort_order"] = "7"
 
                tConditionals["14"] = {}
                tConditionals["14"]["name"] = "NUMBER_LIGHT_LEVEL"
                tConditionals["14"]["type"] = "NUMBER"
                tConditionals["14"]["condition_statement"] = "[14-NUMBER-DYNAMIC] Light Level"
                tConditionals["14"]["description"] = "[14] NAME is LOGIC INTEGER"
                tConditionals["14"]["minimum"] = "10"
                tConditionals["14"]["maximum"] = "150"
                tConditionals["14"]["sort_order"] = "9"
 
                tConditionals["15"] = {}
                tConditionals["15"]["name"] = "STRING_LIGHT"
                tConditionals["15"]["type"] = "STRING"
                tConditionals["15"]["condition_statement"] = "[15-STRING-DYNAMIC] Light Level"
                tConditionals["15"]["description"] = "[15] NAME Level is LOGIC STRING"
                tConditionals["15"]["sort_order"] = "11"
 
                tConditionals["16"] = {}
                tConditionals["16"]["name"] = "LIST_LIGHT_LEVEL"
                tConditionals["16"]["type"] = "LIST"
                tConditionals["16"]["condition_statement"] = "[16-LIST-DYNAMIC] Light Level"
                tConditionals["16"]["description"] = "[16] NAME is LOGIC STRING"
                tConditionals["16"]["list_items"] = "10%,20%,30%,40%,50%,60%,70%,80%,90%,100%"
                tConditionals["16"]["sort_order"] = "13"
 
                tConditionals["17"] = {}
                tConditionals["17"]["name"] = "ROOM_SELECTION"
                tConditionals["17"]["type"] = "ROOM"
                tConditionals["17"]["condition_statement"] = "[17-ROOM-DYNAMIC] Room Selection is"
                tConditionals["17"]["description"] = "[17] NAME is LOGIC STRING"
                tConditionals["17"]["sort_order"] = "15"
 
                tConditionals["18"] = {}
                tConditionals["18"]["name"] = "DEVICE_SELECTION"
                tConditionals["18"]["type"] = "DEVICE"
                tConditionals["18"]["condition_statement"] = "[18-DEVICE-DYNAMIC] Device Selection is     (dynamic)"
                tConditionals["18"]["description"] = "[18] NAME Device Selection LOGIC DEVICE"
                tConditionals["18"]["sort_order"] = "17"
 
                return tConditionals
end
```


The result of ordering both the static and dynamic conditions found in the code examples is: 

<img src="images/5_1-01.png"/>

You can see that the first conditional displayed is “The Light is on”. This is a static conditional and has a sort order value of 0 ( `<sort_order>0</sort_order>` ) as seen in the xml code Example 1. 

The second conditional displayed is also “The Light is on” however this is the dynamic conditional defined in the lua code Example 2. This conditional has a sort order value of 1 (`tConditionals["10"]["sort_order"] = “1”`)

The third conditional displayed is “The Light is off”, which is a static conditional and has a sort order value of 2 ( `<sort_order>2</sort_order>` ) as seen in the xml code Example 1. 

The remainder of the conditionals in the code examples follow the alternate numbering used between the static XML code and the dynamic .lua code.


### Considerations when Sorting Conditionals

- Sort Order numbering must be a positive number (including 0). The list is sorted in numerical order.
-  If a sort order number value is used twice, the second Conditional using the same value will not be displayed.
- When defining Conditionals that will use the Sort Order element, all of the Conditionals must include a numeric value. Failing to provide a sort order number for one Conditional will result in none of the Conditionals being sorted. For example, if you have ten Conditionals defined in your driver and one of them is missing the sort order element - none of the Conditionals will be sorted.
