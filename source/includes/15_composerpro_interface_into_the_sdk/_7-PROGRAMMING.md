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

**Note that a limit of 1000 Events per driver exists within the Control4 Operating System. Exceeding this limit results in undesirable handling of Events.**

### Sorting Events in ComposerPro

Prior to the release of O.S. 3.3.0, driver writers did not have the ability to order the way driver Events were displayed in the Programming area of ComposerPro. A sort order driver provides the ability to display Events in any desired order. Sort order can be applied to Events in the driver.xml with the inclusion of the following xml tag: \<sort\_order\>\</sort\_order\>. See the lighting code Example 1 from a driver.xml file to the right.

The ability to order the way driver Events are displayed in the Properties area of ComposerPro is possible with the use of a sort order driver XML tag:

```xml
            <event>
                <name>New Event</name>
                <description>New Description</description>
                <sort_order>0</sort_order>
            </event>
            <event>
                <name>New Event 1</name>  
                <description>Event Description 1</description>
                <sort_order>1</sort_order>
            </event>
            <event>
                <name>New Event 2</name>
                <description>Event Description 2</description>
                <sort_order>2</sort_order>
            </event>
            <event>
                <name>New Event 3</name>
                <description>Event Description 3</description>
                <sort_order>3</sort_order>
            </event>
            <event>
                <name>New Event 4</name>
                <description>Event Description 4</description>
                <sort_order>4</sort_order>
            </event>
            <event>
                <name>New Event 5</name>
                <description>Event Description 10</description>
                <sort_order>5</sort_order>
            </event>
```

`<sort_order></sort_order>` 

The tag accepts a zero based list and will display the Events in the order of the number included in each of the Events’ `<sort_order></sort_order>`tag.

For example, in the XML code to the right there are five device specific Events. They will display in ComposerPro Programming as:

- New Event
- New Event 1
- New Event 2
- New Event 3
- New Event 4
- New Event 5
