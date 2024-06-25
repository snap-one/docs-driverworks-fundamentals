## Advanced Property Types

**LABEL**
Prior to OS Release 2.10.0, the only way to create a separator or section between properties in the driver was to use a STRING type property and mark it read only. Now, with the inclusion of the XML example to the right, a much cleaner header can be created:

```xml
<config>
  <properties>
   <property>
    <name>Header</name>
    <type>LABEL</type>
   <default>New Header</default>
  </property>
 </properties>
</config>
```

An example of the new Header are on the Properties screen in ComposerPro:

<img src="images/15_3-01.png"/>


Note that this PropertyType could also be used to make a blank space by omitting the \<default\>\</default\> element.



**Scroll**

Prior to OS Release 2.10.0, the only way to set an integer value on a scale was to have a `RANGED_INTEGER` type property. Now, with the inclusion of the XML to the right, a more graphical view can be provided.

```xml
<config>
  <properties>
   <property>
    <name>New Scroll Control</name>
    <type>SCROLL</type>
    <minimum>0</minimum>
    <maximum>60</maximum>
    <default>0</default>
   </property>
  </properties>
</config>
```

An example of the new Scroll Property Type on the Properties screen is shown above,




**TRACK**

A second option is available for setting an integer value which is similar to Scroll. Track is more of a discrete slider control. The XML for Track is also to the right:

```xml
<config>
   <properties>
    <property>
     <name>New Scroll Control</name>
     <type>TRACK</type>
     <minimum>0</minimum>
     <maximum>10</maximum>
     <default>0</default>
    </property>
   </properties>
</config>
```

An example of the new Scroll Property Track on the Properties screen is shown above




