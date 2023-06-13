

## Sorting Events in ComposerPro

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