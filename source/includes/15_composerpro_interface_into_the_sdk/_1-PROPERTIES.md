## Properties

DriverWorks Properties are defined in the .c4z file and exposed in the Composer System Design interface on the Properties tab. Below is table of currently supported properties. Code examples for each are to the right. 

| Property | Description |
| --- | --- |
| **List** | Provides a list of selectable objects. |
| **Ranged Float** | Provides a range of selectable floating-point or non-integer numbers. |
| **Ranged Integer** | Provides a range of selectable integers. |
| **String** | Provides a read only string. |
| **String Password** | Provides a string which can be used as a password. |
| **Device Selector** | Provides a list of selectable drivers. See the Device Selector topic for more information. |
| **Color Selector** | Provides the ability to select a color. See the Color Selector topic for more information. |
| **Dynamic List** |  Provides driver-based, dynamically updated lists. See the Dynamic List topic for more information. |
| **Link** | Provides a link to a resource which can be used to support your driver. |

```xml
List Property Example
<property>
       <name>Log Level</name>
        <type>LIST</type>
          <readonly>false</readonly>
          <default>2 - Warning</default>
             <items>
               <item>0 - Fatal</item>
               <item>1 - Error</item>
               <item>2 - Warning</item>
               <item>3 - Info</item>
               <item>4 - Debug</item>
               <item>5 - Trace</item>
            </items>
</property>
```




```xml
Ranged Float Property Example
<property>
        <name>My Float</name>
        <type>RANGED_FLOAT</type>
        <minimum>5.0</minimum>
        <maximum>50.0</maximum>
        <default>10.0</default>
        <readonly>false</readonly>
</property>
```





```xml
Ranged Integer Property Example
<property>
    <name>Zone</name>
     <type>RANGED_INTEGER</type>
     <minimum>1</minimum>
     <maximum>8</maximum>
     <default>1</default>
     <readonly>false</readonly>
     <tooltip>Zone number of the thermostat.</tooltip>
</property>
```




```xml
String Property Example
<property>
  	<name>Test Read Only</name>
  	<type>STRING</type>
  	<readonly>true</readonly>
  	<tooltip>Property to test Read Only Property Tooltip</tooltip>
  	<description>This is a test description.</description>
  	<default>ReadOnly</default>
</property>
```




```xml
String Paswword Property Example
<property>
  <name>Test STRING password</name>
  <type>STRING</type>
  <password>true</password>
  <default>password</default>
</property>
```




```xml
Device Selector Property Example
<property>
    <name>AssociatedDevice</name>
    <type>DEVICE_SELECTOR</type>_
   <items>
       <item>control4_network_mediastorage.c4i</item>
     </items>
     <multiselect>false</multiselect>
</property>
```




```xml
Color Selector Property
<property>
    <name>Test Color Selector</name>
    <type>COLOR_SELECTOR</type>
    <default>255,0,0</default>
</property>
```




```xml
Dynamic List Property Example
<property> 
   <name>Test Dynamic List No Default</name> 
    <type>DYNAMIC_LIST</type> 
    <readonly>false</readonly> 
    <tooltip>DYNAMIC_LIST No Default Property Tooltip</tooltip>
</property>
```



```html
Link Property Example
<property>
   <name>Test Link</name>
   <type>LINK</type>
   <tooltip>Property to test Link Property Tooltip</tooltip>
   <default>http://www.control4.com</default>
</property>
```



### Usage Note Regarding the Ranged Float Property
Note that the local setting of a Control4 Controller in a project, and the local setting assigned to devices found within that project can potentially have an impact of the way in which a RANGED FLOAT property is handled by a driver. For example, if the controller has a local setting which uses the English language, the RANGED FLOAT property will be defined as in the example to the right (using a decimal).  However, if the local setting is that which supports another language - such as French for example, the minimum, maximum and default values will come in using a comma instead of a decimal point. For example, 50.0 will be received as 50,0. In the event that project controllers and project devices cannot be set to the same local, resulting in the same language, a driver will need to be able to handle the differing value definitions. 

### Usage Note Regarding the Password Property
The Password property type prevents the text of a password from being displayed. It will show asterisks `*` in place of any text that is part of the property.  It is important to note that the data of the password field is NOT protected in any way. For example, if the properties table from the driver were to be printed, it will show the actual password. If the password field is used by a driver Control4 strongly recommends that the driver be encrypted.


### Using tooltips and Descriptions in Properties
All properties can have initial/default values and may be changed by the installer or configured as read-only.

Properties have the ability to have two parameters. These include \<tooltip\> and \<description\>.

The \<tooltip\> parameter provides the means to display a brief description of the parameter when the user hovers over it with their mouse. 

For example:

<img src="images/15_1-01.png"/>

The `<description>` parameter contains text that can be displayed in the description field to provide additional information about the property.


For example:

<img src="images/15_1-02.png"/>


In the Tooltip/Description Example to the right, the two parameters are passed to the string property named TEST READ ONLY.

```xml
Tooltip/Description Example

<property>
    <name>Test Read Only</name>
     <type>STRING</type>
     <readonly>true</readonly>
     <tooltip>Property to test Read Only Property Tooltip</tooltip>
     <description>This is a test description.</description>
     <default>ReadOnly</default>
 </property>
```


<img src="images/15_1-03.png"/>

