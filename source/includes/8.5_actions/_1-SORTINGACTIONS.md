## Sorting Actions in ComposerPro

Prior to the release of O.S. 3.3.0, driver writers did not have the ability to order the way driver Actions were displayed in the Programming area of ComposerPro. A new sort order driver element has been delivered that provides the ability to display Actions in any desired order. Sort order can be applied to Actions in the driver.xml with the inclusion of the following xml tag: `\<sort\_order\>\</sort\_order\>`. See the lighting code Example 1 from a driver.xml file to the right.


```xml
<action>
	<name>New_Action</name>
	<command>New_Action</command>
	<sort_order>0</sort_order>
</action>
<action>
	<name>New_Action_1</name>
	<command>New_Action1</command>
	<sort_order>1</sort_order>
</action>
<action>
	<name>New_Action_2</name>
	<command>New_Action2</command>
	<sort_order>2</sort_order>
</action>
<action>
	<name>New_Action_3</name>
	<command>New_Action3</command>
	<sort_order>3</sort_order>
</action>
<action>
	<name>New_Action_4/name>
	<command>New_Action4</command>
	<sort_order>4</sort_order>
</action>
<action>
	<name>New_Action_5/name>
	<command>New_Action5</command>
	<sort_order>5</sort_order>
</action>
```

`<sort_order></sort_order>` 

The tag accepts a zero based list and will display the Actions in the order of the number included in each Actions’ `<sort_order></sort_order>`tag.

For example, in the XML code to the right there are five device specific Actions. They will display under the Actions tab in ComposerPro’s Property screen as:

- New Action
- New Action 1
- New Action 2
- New Action 3
- New Action 4
- New Action 5
