##  Understanding XML in a NUMBER Type Conditional

In this section, we’ll define a NUMBER type conditional called `NUMBER_LIGHT_LEVE`L. NUMBER type conditionals compare the current device value with an integer entered in ComposerPro programming by the integrator. This comparison can use the following operators: =, !=. Here is the example XML code used to define this type of conditional:

`34.  <conditional>`
`35.     <id>4</id>`
`36.     <name>NUMBER_LIGHT_LEVEL</name>`
`37.     <type>NUMBER</type>`
`38.     <condition_statement>Light Level</condition_statement>`
`39.     <description>NAME is LOGIC INTEGER</description>`
`40.     <minimum>10</minimum>`
`41.     <maximum>150</maximum>`
`42.   </conditional>`

Line 34: `<conditional></conditional>`
XML tag for this unique conditional.

Line 35: `<id></id>`
The conditional’s numeric ID value. It is recommended that conditionals be numbered beginning with 0. All conditionals require a unique numeric ID.

Line 36: `<name></name>`
The name of the conditional. This is a unique name defined by the driver writer. It is used in the TestCondition function in the Lua code of the driver. This is given as the first parameter to the TestCondition function. This is how the driver knows what condition is being tested.

Line 37: `<type></type>`
The type of the conditional as defined above in the Conditional Types section.

Line 38: `<condition_statement></condition_statement>` 
The conditional statement states a question being asked regarding a state of the driver. This is the statement that is shown before the condition logic. For example, in the `NUMBER_LIGHT_LEVEL` conditional above the question being asked is: Is the device value equal to or not equal to a numeric value entered in ComposerPro programming by the integrator. It is basically a useful statement that supports the conditional. When an integrator is viewing this driver’s conditionals in ComposerPro, they can enter a numeric value for this conditional and ultimately initiate programming when the device state is equal to or not equal to the number entered. 

Line 39: `<description></description>`
The description element needs to contain the NAME macro. NAME is a key value that is equivalent to your driver’s unique name. Every conditional needs to include NAME. ComposerPro treats the NAME macro in your conditional XML in a way that replaces it with your driver’s unique name. The unique name is formatted by the name of the room where the driver resides followed by “-\>”, followed by the name of the driver as it is named in the project. For example, consider that the name of our example driver is “Living Room Lamp” and it is in the Living Room. Based on this, “Living Room-\>Living Room Lamp” will replace NAME when your conditional programming description is displayed in the conditionals window and the script actions area of ComposerPro. Using the example above, ComposerPro can display the description XML element as: “If Living Room-\>Living Room Lamp is = 75%” where the numeric value of 75 is entered by the integrator. Note the use of the LOGIC STRING statement in the description XML. This provides the ability for an integrator to select two of the logic operators and apply that operator to the selected value. Operators for the STRING type conditional include: =, !=

