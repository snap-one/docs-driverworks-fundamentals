## Simple Conditional

### Understanding XML in a SIMPLE Type Conditional

Your conditional XML code must be defined in a manner that can be used by ComposerPro. In this section, we’ll define a SIMPLE type conditional called `SIMPLE_LIGHT_ON`. In doing so we’ll look at each line of XML required to define this type of conditional:

`1.  <conditionals>`
`2.   <conditional>`
`3.     <id>0</id>`
`4.     <name>SIMPLE_LIGHT_ON</name>`
`5.     <type>SIMPLE</type>`
`6.     <condition_statement>The light is on</condition_statement>`
`7.     <description>NAME is On</description>`
`8.   </conditional>`
`9.  </conditionals>`

Line 1: `<conditionals></conditionals> `
This is the driver’s root XML tag that contains all the driver’s conditional code.

Line 2: `<conditional></conditional> `
XML tag for this unique conditional.

Line 3: `<id></id>`
The conditional’s numeric ID value. It is recommended that conditionals be numbered beginning with 0. All conditionals require a unique numeric ID.

Line 4: `<name></name>`
The name of the conditional. This is a unique name defined by the driver writer. It is used in the TestCondition function in the Lua code of the driver. This is given as the first parameter to the TestCondition function. This is how the driver knows what condition is being tested.

Line 5: `<type></type>`
The type of the conditional as defined above in the Conditional Types section.

Line 6: \<`condition_statement></condition_statement>` 
The conditional statement states a question being asked regarding a state of the driver. This is the statement that is shown before the condition logic. For example, the Simple Light On conditional above, the question being asked is if ‘The Light is on’. It is basically a useful statement that supports the conditional. When an integrator is viewing this driver’s conditionals in ComposerPro, they can select “The Light is on” from the list of conditionals and can ultimately initiate programming when the light is in the On state.

Line 7: `<description></description>`
The description element needs to contain the NAME macro. NAME is a key value that is equivalent to your driver’s unique name. Every conditional needs to include NAME. ComposerPro treats the NAME macro in your conditional XML in a way that replaces it with your driver’s unique name. The unique name is formatted by the name of the room where the driver resides followed by “-\>”, followed by the name of the driver as it is named in the project. For example, consider that the name of our example driver is “Living Room Lamp” and it is in the Living Room. Based on this, “Living Room-\>Living Room Lamp” will replace NAME when your conditional programming description is displayed in the conditionals window and the script actions area of ComposerPro. Using the example above, ComposerPro will display the description XML element as: “If Living Room-\>Living Room Lamp is on”.
