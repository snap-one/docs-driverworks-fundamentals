## Conditional XML Example Code

Previously, we discussed how to define a SIMPLE type conditional. The remainder of this document will explain each of the conditional types listed above in the same manner. An XML code set defining these conditionals is referenced throughout. The conditional code examples in the XML are based on an example Light driver. This "driver code” is for explanation purposes only. Typically, the Light Proxy would be used in this case. However, this example explains how Conditionals are not only defined but can be useful for drivers that need to offer them when they are not provided in the scope of a Control4 Proxy. The sample XML from the “driver” is as follows:

	1. <conditionals>
	2.   <conditional>
	3.     <id>0</id>
	4.     <name>SIMPLE_LIGHT_ON</name>
	5.     <type>SIMPLE</type>
	6.     <condition_statement>The light is on</condition_statement>
	7.     <description>NAME is On</description>
	8.   </conditional>
	9.   <conditional>
	10.     <id>1</id>
	11.     <name>SIMPLE_LIGHT_OFF</name>
	12.     <type>SIMPLE</type>
	13.     <condition_statement>The light is off</condition_statement>
	14.     <description>NAME is Off</description>
	15.   </conditional>
	16.   <conditional>
	17.     <id>2</id>
	18.     <name>BOOL_LIGHT</name>
	19.     <type>BOOL</type>
	20.     <condition_statement>Light is</condition_statement>
	21.     <description>NAME Light is STRING</description>
	22.     <true_text>On</true_text>
	23.     <false_text>Off</false_text>
	24.   </conditional>
	25.   <conditional>
	26.     <id>3</id>
	27.     <name>BOOL_LIGHT_ON</name>
	28.     <type>BOOL</type>
	29.     <condition_statement>Light is On</condition_statement>
	30.     <description>NAME Light is On is STRING</description>
	31.     <true_text>True</true_text>
	32.     <false_text>False</false_text>
	33.  </conditional>
	34.  <conditional>
	35.     <id>4</id>
	36.     <name>NUMBER_LIGHT_LEVEL</name>
	37.     <type>NUMBER</type>
	38.     <condition_statement>Light Level</condition_statement>
	39.     <description>NAME is LOGIC INTEGER</description>
	40.     <minimum>10</minimum>
	41.     <maximum>150</maximum>
	42.   </conditional>
	43.   <conditional>
	44.     <id>5</id>
	45.     <name>STRING_LIGHT</name>
	46.     <type>STRING</type>
	47.     <condition_statement>Light Level</condition_statement>
	48.     <description>NAME Level is LOGIC STRING</description>
	49.   </conditional>
	50.   <conditional>
	51.     <id>6</id>
	52.     <name>LIST_LIGHT_LEVEL</name>
	53.     <type>LIST</type>
	54.     <condition_statement>Light Level</condition_statement>
	55.     <description>NAME is LOGIC STRING</description>
	56.     <items>
	57.      <item>10%</item>
	58.      <item>20%</item>
	59.      <item>30%</item>
	60.      <item>40%</item>
	61.      <item>50%</item>
	62.      <item>60%</item>
	63.      <item>70%</item>
	64.      <item>80%</item>
	65.      <item>90%</item>
	66.      <item>100%</item>
	67.    </items>
	68.  </conditional>
	70.  <conditional>
	71.    <id>7</id>
	72.    <name>ROOM_SELECTION</name>
	73.    <type>ROOM</type>
	74.    <condition_statement>[7-ROOM] Room Selection is</condition_statement>
	75.    <description>[7] NAME Room Selection LOGIC ROOM</description>
	76.  </conditional>
	77.  <conditional>
	78.    <id>8</id>
	79.    <name>DEVICE_SELECTION</name>
	80.    <type>DEVICE</type>
	81.    <condition_statement>[8-DEVICE] Device Selection is</condition_statement>
	82.    <description>[8] NAME Device Selection LOGIC DEVICE</description>
	83.  </conditional>
	84.</conditionals>
	
Note: The order in which the conditionals defined in the XML will be reversed in their order of display in ComposerPro. For example, above the` SIMPLE_LIGHT_ON` conditional is defined first. It will display at the bottom of the conditional list in the Programming area of ComposerPro.
