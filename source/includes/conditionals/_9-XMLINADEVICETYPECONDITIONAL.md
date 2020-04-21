## Understanding XML in a DEVICE Type Conditional

In this section, we’ll define a Device type conditional called `DEVICE_SELECTION`. The DEVICE type conditional provides the ability to initiate programming based on whether or not a device selection is equal to or not equal to a device in the project. Programming is completed by the integrator through selecting a device from the drop-down list in Conditionals tab. The list will contain all of the Rooms in the project and their respective devices. Next, the integrator selects whether or not programming will be executed if the device selection is equal to or not equal to that device. This conditional uses the following operators: =, !=.

Here is the example XML code used to define this type of conditional:

`77.  <conditional>`
`78.    <id>8</id>`
`79.    <name>DEVICE_SELECTION</name>`
`80.    <type>~DEVICE~</type>`
`81.    <condition_statement>[8-DEVICE] Device Selection is</condition_statement>`
`82.    <description>[8] NAME Device Selection LOGIC DEVICE</description>`
`83.  </conditional>`


Line 77: `<conditional></conditional>`
XML tag for this unique conditional.

Line 78: `<id></id>`
The conditional’s numeric ID value. It is recommended that conditionals be numbered beginning with 0. All conditionals require a unique numeric ID.

Line 79: `<name></name>`
The name of the conditional. This is a unique name defined by the driver writer. It is used in the TestCondition function in the Lua code of the driver. This is given as the first parameter to the TestCondition function. This is how the driver knows what condition is being tested.

Line 80: `<type></type>`
The type of the conditional as defined above in the Conditional Types section.

Line 81: `<condition_statement></condition_statement>`
The conditional statement states a question being asked regarding a state of the driver. This is the statement that is shown before the condition logic. For example, in the `DEVICE_SELECTION` conditional above, the question being asked is: Is the device value equal to or not equal to the device value selected in ComposerPro programming by the integrator. It is basically a useful statement that supports the conditional. When an integrator is viewing this driver’s conditionals in ComposerPro, they can select a device from the project and ultimately initiate programming when the device is equal to or not equal to the device selected in the Conditionals programming tab.

Line 82: \<description\>\</description\>
The description element needs to contain the NAME macro. NAME is a key value that is equivalent to your driver’s unique name. Every conditional needs to include NAME. ComposerPro treats the NAME macro in your conditional XML in a way that replaces it with your driver’s unique name. The unique name is formatted by the name of the room where the driver resides followed by “-\>”, followed by the name of the driver as it is named in the project. For example, consider that our driver name is Test Driver and the integrator selected a device named Light along with the not equals to operator. The Conditional statement formed in ComposerPro programming would be: If Theater -\> Device Selection != Light


Note the use of the LOGIC STRING statement in the description XML. This provides the ability for an integrator to select two of the logic operators and apply that operator to the selected value. Operators for the Room type conditional include: =, !=.