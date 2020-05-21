## Understanding XML in a BOOLEAN Type Conditional

In this section, we’ll define a Boolean type conditional called `BOOL_LIGHT`. BOOLEAN type conditionals ask a True/False question based on True/False text provided by the driver conditional configuration. Programming is completed by the integrator responding to the True/False question by making response selections in the programming window. Here is the example XML code used to define this type of conditional:

```xml
16.   <conditional>
17.     <id>2</id>
18.     <name>BOOL_LIGHT</name>
19.     <type>BOOL</type>
20.     <condition_statement>Light is</condition_statement>
21.     <description>NAME Light is STRING</description>
22.     <true_text>On</true_text>
23.     <false_text>Off</false_text>
24.   </conditional>
```

Line 16: `<conditional></conditional>`
XML tag for this unique conditional.

Line 17: `<id></id>`
The conditional’s numeric ID value. It is recommended that conditionals be numbered beginning with 0. All conditionals require a unique numeric ID.

Line 18: `<name></name>`
The name of the conditional. This is a unique name defined by the driver writer. It is used in the TestCondition function in the Lua code of the driver. This is given as the first parameter to the TestCondition function. This is how the driver knows what condition is being tested.

Line 19: `<type></type>`
The type of the conditional as defined above in the Conditional Types section.

Line 20: `<condition_statement></condition_statement>` 
The conditional statement states a question being asked regarding a state of the driver. This is the statement that is shown before the condition logic. For example, in the `BOOL_LIGHT`conditional above, the question being asked involves whether or not the Light is On or Off. It is basically a useful statement that supports the conditional. When an integrator is viewing this driver’s conditionals in ComposerPro, they can select an On or Off value and ultimately initiate programming based on that selection. 

Line 21: `<description></description>`
The description element needs to contain the NAME macro. NAME is a key value that is equivalent to your driver’s unique name. Every conditional needs to include NAME. ComposerPro treats the NAME macro in your conditional XML in a way that replaces it with your driver’s unique name. The unique name is formatted by the name of the room where the driver resides followed by “-\>”, followed by the name of the driver as it is named in the project. For example, consider that the name of our example driver is “Living Room Lamp” and it is in the Living Room. Based on this, “Living Room-\>Living Room Lamp” will replace NAME when your conditional programming description is displayed in the conditionals window and the script actions area of ComposerPro. Using the example above, ComposerPro can display the description XML element as: “If Living Room-\>Living Room Lamp is Off” where the Off value was selected by the integrator. 

Line 22: \<`true_text></true_text>`
Line 23: \<`false_text></false_text>`
Note the use of the `<*_text>` XML elements in this example. The values entered here will be the responses to the True/False questions asked by the conditional. Note that these values can be any string value. For example, here is another BOOLEAN conditional taken from the code sample above. It is called `BOOL_LIGHT_ON` and asks the question “If Living Room-\>Living Room Lamp is On”. The integrator can then select either of the text XML elements of “True” and “False” to complete programming.

`25.   <conditional>`
`26.     <id>3</id>`
`27.     <name>BOOL_LIGHT_ON</name>`
`28.     <type>BOOL</type>`
`29.     <condition_statement>Light is On</condition_statement>`
`30.     <description>NAME Light is On is STRING</description>`
`31.     <true_text>True</true_text>`
`32.     <false_text>False</false_text>`
`33.   </conditional>`
