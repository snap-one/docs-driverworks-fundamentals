## Understanding XML in a ROOM Type Conditional

In this section, we’ll define a Room type conditional called `ROOM_SELECTION`. The ROOM type conditional provides the ability to initiate programming based on whether or not a room selection is equal to or not equal to a room in the project. Programming is completed by the integrator through selecting a room from the drop-down list in Conditionals tab. The list will contain all of the Rooms defined in the project. Next, the integrator selects whether or not programming will be executed if the room selection is equal to or not equal to that room. This conditional uses the following operators: =, !=.

Here is the example XML code used to define this type of conditional:

`70.  <conditional>`
`71.    <id>7</id>`
`72.    <name>ROOM_SELECTION</name>`
`73.    <type>ROOM</type>`
`74.    <condition_statement>[7-ROOM] Room Selection is</condition_statement>`
`75.    <description>[7] NAME Room Selection LOGIC ROOM</description>`
`76.  </conditional>`


Line 70: `<conditional></conditional>`
XML tag for this unique conditional.

Line 71: `<id></id>`
The conditional’s numeric ID value. It is recommended that conditionals be numbered beginning with 0. All conditionals require a unique numeric ID.

Line 72: `<name></name>`
The name of the conditional. This is a unique name defined by the driver writer. It is used in the TestCondition function in the Lua code of the driver. This is given as the first parameter to the TestCondition function. This is how the driver knows what condition is being tested.

Line 73: `<type></type>`
The type of the conditional as defined above in the Conditional Types section.

Line 74: `<condition_statement></condition_statement>`
The conditional statement states a question being asked regarding a state of the driver. This is the statement that is shown before the condition logic. For example, in the `ROOM_SELECTION` conditional above, the question being asked is: Is the room value equal to or not equal to the room value selected in ComposerPro programming by the integrator. It is basically a useful statement that supports the conditional. When an integrator is viewing this driver’s conditionals in ComposerPro, they can select a room from the project and ultimately initiate programming when the room is equal to or not equal to the room selected in the Conditionals programming tab.

Line 75: `<description></description>`
The description element needs to contain the NAME macro. NAME is a key value that is equivalent to your driver’s unique name. Every conditional needs to include NAME. ComposerPro treats the NAME macro in your conditional XML in a way that replaces it with your driver’s unique name. The unique name is formatted by the name of the room where the driver resides followed by “-\>”, followed by the name of the driver as it is named in the project. For example, consider that our driver name is Test Driver and the integrator selected a room named Theater along with the equals operator. The Conditional statement formed in ComposerPro programming would be: If Theater -\> Room Selection = Theater

Note the use of the LOGIC STRING statement in the description XML. This provides the ability for an integrator to select two of the logic operators and apply that operator to the selected value. Operators for the Room type conditional include: =, !=.
