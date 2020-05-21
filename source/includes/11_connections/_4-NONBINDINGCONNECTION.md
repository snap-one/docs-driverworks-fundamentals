## Non-Binding Connection

The traditional connection in Composer represents a cable that is attached from the output of one device to the input of another device. A non-binding connection lacks the physical cable and is used with devices that are not intended to be connected by the dealer in Composer. Likewise, a non-binding connection does not require a dealer to identify the connection. Non-binding connections are beneficial as they remove the burden of Director and Composer to maintain bindings that don't actually exist. Their use makes the model in Director more accurately reflect the real world. In many cases these are web based connections between devices that are not connected by a cable. 

For example, consider an IP Video Matrix which has the following connection path:

BlueRay -\> HDMI Server \<\>--Non-Binding Connection--\<\> HDMI Client -\> TV -\> Room Video Endpoint

In this example, the BlueRay will appear as a video source in the project's TV Room.

Characteristics of a non-binding connections include:

- Any provider is connected to all consumers. 
- The non-binding connection has a key of datatype text, which discriminates which connections are connected. 
- All non-binding providers with a key are connected to all non-binding consumers with the same key. 
- There is no requirement for the dealer to establish the connections in Composer.

A non-binding connection is created in the `<connection`\> section of the the driver. The included is a Provider-side example of a non-binding connection. Note the use of the `<nonbindingkey>` element. See the example to the right:

```xml
<connection>
 <id>3333</id>
 <connectionname>Net Output</connectionname>
 <type>6</type>
 <consumer>False</consumer>
 <audiosource>True</audiosource>
 <videosource>False</videosource>


 <hidden>True</hidden>
 <nobindingkey>Connection Key</nobindingkey>


 </classes>
  <class>
   <classname>SOME_CLASS</classname>
  </class>

 </classes>
</connection>
```

Next is the Consumer-side example for the non-binding connection. Note the use of the `<nonbindingkey>` element:

```xml
<connection>
 <id>4444</id>
 <connectionname>Net Input</connectionname>
 <type>6</type>
 <consumer>True</consumer>
 <audiosource>False</audiosource>
 <videosource>False</videosource>

 <hidden>True</hidden>
 <nobindingkey>Connection Key</nobindingkey>

 </classes>
  <class>
   <classname>SOME_CLASS</classname>
  </class>
 </classes>
</connection>
```

In both examples to the right, the connection uses the hidden element set to True as typically a non-binding connection should not be displayed.

