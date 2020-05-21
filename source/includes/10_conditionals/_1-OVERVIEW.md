## Overview

Driver conditionals are useful to include in your driver as they provide the opportunity for custom programming through the interface found in ComposerPro. When considering conditionals, it’s helpful to think of them as “If” programming statements. For example, a light driver may have a simple conditional defined for when the light is in the On state. Using this conditional can then support programming in the project that will be executed if the light is On.

Conditionals are defined in the driver’s XML area within the conditionals tag. See the example to the right:

```xml
1.  <conditionals>
2.   <conditional>
3.     <id>0</id>
4.     <name>SIMPLE_LIGHT_ON</name>
5.     <type>SIMPLE</type>
6.     <condition_statement>The light is on</condition_statement>
7.     <description>NAME is On</description>
8.   </conditional>
9.  </conditionals>
```


### Conditional Types

Conditionals are not just limited to the simple example provided above. Currently there are seven types of conditionals that can be included in your driver. They vary in complexity and each are intended to provide a specific programming opportunity. They include:

**LIST**: Asks if the current device value is equal to or not equal to the selected item in a list of values defined in the driver. These values can be numbers or strings.

**STRING**: Asks if the current device value provided in string format is equal to or not equal to a string value entered in ComposerPro programming by the integrator.

**NUMBER**: Uses the following operators: =, !=, \<, \<=, \>, \>= to compare the current device value with an integer entered in ComposerPro programming by the dealer.

**BOOLEAN**: Asks a True/False question based on True/False text provided by the driver conditional configuration.

**SIMPLE**: Asks a true or false question.

**ROOM**: Provides the ability to initiate programming based on whether or not a room selection is equal to or not equal to a room in the project.

**DEVICE**: Provides the ability to initiate programming based on whether or not a device selection is equal to or not equal to a device in the project.

A Note about Loops
Before detailing each of the conditionals listed above, it’s worth noting the role of Loops in ComposerPro programming. Loops offer another opportunity of custom programming for your driver. ComposerPro will use the XML for the conditionals you’ve defined in your driver and replicate them under their own Loops programming tab. All of your conditionals, and their respective functionality, are listed under the Loops tab exactly as they are under the conditionals tab. However, Loops differ from conditionals in that they can be considered as “While” programming statements. 

Above we looked at a simple light on conditional example. When ComposerPro creates a Loop based on this conditional, it provides a programming opportunity that can be used while the light is in the On state. Using this Loop supports programming in the project that will be executed while the light is On.

As mentioned above, ComposerPro created these Loops from your driver’s conditional XML code. As a driver writer, you need to do nothing for Loops to be available for use in ComposerPro. However, it is worth considering that Loops can be resource expensive as they will continue to execute throughout the duration that a device is in a certain state. If your conditional code lends itself to the potential of this happening addressing this in your driver documentation is strongly recommended.
