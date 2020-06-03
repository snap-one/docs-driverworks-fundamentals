## Dynamic List Properties

The ability to provide driver-based, dynamically updated lists in ComposerPro’s Advanced Properties screen has been included in DriverWorks. When this feature is included in a driver’s properties section, the Properties tab in ComposerPro will display a list of items and (optionally) a default item for the list can be displayed. 

The following sample property code uses the new property called `DYNAMIC_LIST`.

```xml
<property>
  <name>Test Dynamic List</name>
  <type>DYNAMIC_LIST</type>
  <default>Item 5</default>
  <tooltip>DYNAMIC\_LIST Property Tooltip\</tooltip>
</property>
```

In the example above, a name for the property is included as “Test Dynamic List”. This is the string value that will be displayed as the name for this list in ComposerPro. Next, you can see the property type of `DYNAMIC_LIST`.  Next we can see that the selected parameter has a value of “Item5”. This will result in Item 5 being displayed as the default value (displayed value) when the list is shown in ComposerPro. Finally, the optional tooltip parameter is populated with text that is displayed when the user hovers over the Test Dynamic List field in Composer Pro. The resulting List can be seen as:

<img src="images/15_2-01.png"/>

In the above screen shot, we can see the Test Dynamic List displayed along with its selected value of Item 5. The selected value for the list can be changed from within ComposerPro by selecting the new default value and clicking on the Set button.

The `DYNAMIC_LIST` property can also be configured to use no default value. An example of the XML for that type of list would be:

```xml
<property>
 <name>Test Dynamic List No Default</name>
 <type>DYNAMIC_LIST</type>
 <readonly>false</readonly>
 <tooltip>DYNAMIC_LIST No Default Property Tooltip</tooltip>
</property>
```


The ComposerPro Properties Page would look like this:

<img src="images/15_2-02.png"/>


Note that there is no value displayed by default for the list called “Test Dynamic List No Default”. This is due to the omission of the \<default\> parameter. 

List content is delivered from the driver to ComposerPro by using the new UpdatePropertyList API. The API may be called within OnDriverInit. For example:

```lua
function OnDriverInit()
  C4:UpdatePropertyList("Test Dynamic List","Item 4,Item 5,Item 6")
end
```

The example above includes the three items used the “Test Dynamic List” example above.

Here is an example of the API used to populate the items in the second example or the  "Test Dynamic List No Default":

```lua
function OnDriverInit()
  C4:UpdatePropertyList("Test Dynamic List No Default","Item 7,Item 8,Item 9")
end
```

When a user selects an item from the a list’s LOV, OnPropertyChanged is called with the information for the selected item. 

It is important to understand that that Dynamic List items are NOT persisted. The protocol driver must handle rebuilding the list during initialization. This is very different behavior than most other properties.


A sample driver that utilizes Dynamic List Properties is included in the SDK. The driver demonstrates the ability to generate Dynamic Lists based on driver code to support a proxy-less device. The Lists that are generated can subsequently be configured and used in the ComposerPro environment and, through Programming, convey useful information throughout devices running Navigator. Specifically, Dynamic Lists are demonstrated in Properties, Actions and Device Specific Commands.

The driver: `dynamic_list_widget.c4z` can be found in the Samples directory of the SDK in the Dynamic List Properties folder.


