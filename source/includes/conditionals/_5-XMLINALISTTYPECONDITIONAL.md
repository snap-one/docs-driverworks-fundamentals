##  Understanding XML in a LIST Type Conditional

In this section, we’ll define a LIST type conditional called `LIST_LIGHT_LEVEL`. LIST type conditionals provide a list of values which can be selected for use in programming. Here is the example XML code used to define this type of conditional:

`50.   <conditional>`
`51.     <id>6</id>`
`52.     <name>LIST_LIGHT_LEVEL</name>`
`53.     <type>LIST</type>`
`54.     <condition_statement>Light Level</condition_statement>`
`55.     <description>NAME is LOGIC STRING</description>`
`56.     <items>`
`57.      <item>10%</item>`
`58.      <item>20%</item>`
`59.      <item>30%</item>`
`60.      <item>40%</item>`
`61.      <item>50%</item>`
`62.      <item>60%</item>`
`63.      <item>70%</item>`
`64.      <item>80%</item>`
`65.      <item>90%</item>`
`66.      <item>100%</item>`
`67.    </items>`
`68.  </conditional>`


Line 50: `<conditional></conditional>`
XML tag for this unique conditional.

Line 51: `<id></id>`
The conditional’s numeric ID value. It is recommended that conditionals be numbered beginning with 0. All conditionals require a unique numeric ID.

Line 52: `<name></name>`
The name of the conditional. This is a unique name defined by the driver writer. It is used in the TestCondition function in the Lua code of the driver. This is given as the first parameter to the TestCondition function. This is how the driver knows what condition is being tested.

Line 53: `<type></type>`
The type of the conditional as defined above in the Conditional Types section.

Line 54: `<condition_statement></condition_statement>` 
The conditional statement states a question being asked regarding a state of the driver. This is the statement that is shown before the condition logic. For example, the `LIST_LIGHT_LEVEL` conditional above, the question being asked is if the current device value is equal to or not equal to the selected item in a list of values defined in the driver. It is basically a useful statement that supports the conditional. When an integrator is viewing this driver’s conditionals in ComposerPro, they can select a value from the list of values defined in the driver and initiate programming when the light is in the at the level selected.

Line 55: `<description></description>`
The description element needs to contain the NAME macro. NAME is a key value that is equivalent to your driver’s unique name. Every conditional needs to include NAME. ComposerPro treats the NAME macro in your conditional XML in a way that replaces it with your driver’s unique name. The unique name is formatted by the name of the room where the driver resides followed by “-\>”, followed by the name of the driver as it is named in the project. For example, consider that the name of our example driver is “Living Room Lamp” and it is in the Living Room. Based on this, “Living Room-\>Living Room Lamp” will replace NAME when your conditional programming description is displayed in the conditionals window and the script actions area of ComposerPro. Using the example above, ComposerPro can display the description XML element as: “If Living Room-\>Living Room Lamp is 50%” Note the use of the LOGIC STRING statement in the description XML. This provides the ability for an integrator to select any of the logic operators and apply that operator to the selected value. Operators include: =, !=, \<, \<=, \>, \>=. 

Line 56: `<items></items>`
The items element contains a list of values which can be numerical or strings that are displayed in the list in ComposerPro. Integrators can choose a value from the list along with any of the logic operators to complete the programming for the conditional. 
