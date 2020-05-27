## PROGRAMMING


**Commands**
The DriverWorks driver’s commands are used in Control4 programming as device specific commands (device actions are combined with proxy actions) in the Actions dialog for the device. They are “do” statements. Examples of commands include:

- Light: on, off
- Security Device: arm, disarm
- TV device: turn on, turn off, change channel


**Variables**
A variable is a representation of information about the controlled device, such as the device’s current power state. Use the variables section to define the system variables that will be available within the Control4 project that your driver is installed into.  These will be available as system variables to use in programming in the same way that variables from any other device are used. 



**Conditionals**
The use of conditionals in the custom driver is linked with the device variables. It is an ”if” statement. An “if” statement asks a true/false question to the device. Examples of conditionals include:

- If door is open
- If after 5:00 PM
- If light is greater than 50%

See Driver Based Conditionals for additional information.


**Events**
Your driver can trigger events within the Control4 system. Events are used to cause other things to happen within the system using custom programming. They can be considered “when” statements. One of the duties of your driver is to expose all events. Examples of Events include:

- When the door opens.
- When it is 7:00 AM.
- When it is sunrise.