## Non-Binding Connection

Traditionally, a connection in ComposerPro represents a cable that is attached from the output of one device to the input of another device. A non-binding connection lacks the physical cable and is used with devices that are not intended to be connected by the dealer in ComposerPro. Likewise, a non-binding connection does not require a dealer to identify the connection. Non-binding connections are beneficial as they remove the burden of Director and Composer to maintain bindings that don't actually exist. Additionally, their use makes the model in Director more accurately reflect the real world. In many cases these are network based connections between devices that are not connected by a traditional AV cable.

For example, consider an IP Video Matrix which has the following connection path:

BlueRay -\> HDMI Server \<\>--Non-Binding Connection--\<\> HDMI Client -\> TV -\> Room Video Endpoint

In this example, the BlueRay can appear as a video source in the project's TV Room by means of a non-binding connection.

**Characteristics of a non-binding connections include:**

- “Non-binding" refers to the fact that these connections are hidden and, as such, do not require any binding to be made in Composer Pro. 
- "Connection" refers to the fact these connections enable AV Paths to be created between drivers who share the same non-binding connection declaration (aka nonbindingkey). 
- Non-Binding connections create many-to-many path relationships and, as such, are appropriate for modeling devices like media over IP where a project could have many transmitters and receivers located in myriad rooms throughout the project .
- Non-binding connections can mimic AV signal busses and establish signal paths between various devices.
- A non-binding connection has a key of datatype text, which discriminates which connections are connected. 
- All non-binding providers with a key are connected to all non-binding consumers with the same key. 
- There is no requirement for the dealer to establish the non-binding connections in ComposerPro.
- Any provider will create a path to all consumers sharing the same key.


From a driver development perspective, a non-binding connection is created in the `<connection`\> section of the the driver’s XML. The following example is a Provider-side example of a non-binding connection found in the Acme Distributed Media Transmitter sample driver delivered with the SDK. Specifically, this is a non-binding connection for HDMI. 

```xml
</connection>
<connection proxybindingid="5001">
	<id>2201</id>
	<type>5</type>
	<connectionname>Acme MoIP Transmitter Bus</connectionname>
	<consumer>False</consumer>
	<linelevel>False</linelevel>
	<hidden>True</hidden>
	<nobindinggroup>1</nobindinggroup>
	<nobindingkey>ACME_MOIP_TRANSMITTER_BUS</nobindingkey>
	<maxpaths>128</maxpaths>
	<classes>
		<class>
		<classname>HDMI</classname>
		</class>
	</classes>
</connection>
```

Note the use of the following XML elements that are used in non-binding connections:

`<hidden>` - Designates that this binding will not be displayed in ComposerPro.

`<nonbindingkey>` - A string key that establishes many-to-many pathing options with other device drivers using the same key.

`<maxpaths>` - Enforces actual bandwidth/channel limitations. For example, a connection used to model an audio buss capable of 128 channels would have maxpaths set to 128. The Control4 Pathing Engine uses the maxpaths value to determine if a path request can be executed. If the request would exceed the maxpaths then an older path will be stolen to honor the current request.

`<nonbindinggroup>` - Designates the group number of a set of non-binding connections. Controller drivers must be a singleton in a project. If more than one controller is needed then the multiple driver suites (controller, transmitters & receivers) must be developed using the nonbinding group id value to differentiate the systems. _Note that this example is currently out of scope for this documentation._

Next is the Consumer-side example for the HDMI non-binding connection. This is the HDMI connection found in the Distributed Media Controller sample driver delivered with the SDK. Note the use of the `<nonbindingkey>` element value of  `ACME_MOIP_TRANSMITTER_BUS` This element establishes the many-to-many path relationships with other drivers using the same keys. While a non-binding connection is used here, the Pathing Engine will still honor standard Connection Class relationships defined under `<connections>` when determining valid connection paths:

```xml
  ~<connection>~
  <id>1201</id>
  <type>5</type>
  <connectionname>Acme MoIP Transmitter Bus</connectionname>
  <consumer>True</consumer>
  <linelevel>False</linelevel>
  <hidden>True</hidden>
  <nobindinggroup>1</nobindinggroup>
  <nobindingkey>ACME_MOIP_TRANSMITTER_BUS</nobindingkey>
  <maxpaths>128</maxpaths>
  <classes>
    <class>
      <classname>HDMI</classname>
    </class>
  </classes>
</connection>
<connection>
```

In both examples, the connection uses the hidden element set to True as typically a non-binding connection should not be displayed in ComposerPro to avoid confusion.

Beginning with Operating System 4.1.0, the DriverWorks SDK includes support for implementing non-binding connections which includes a suite of sample drivers  and driver documentation to assist with development efforts. The sample drivers can be found here:

[https://github.com/snap-one/docs-driverworks/tree/master/sample\_drivers/nonbinding\_connections][1]

These drivers demonstrate how to model media over IP devices within the Control4 driver architecture.

The sample driver suite includes:

**acme\_distributed\_media\_controller.c4z: **

The example controller driver can be considered the central hub for media distribution throughout the network. In this example, it receives the SET\_INPUT, DISCONNECT\_INPUT and SET\_VOLUME commands from the Receiver and Transceiver drivers. These commands are sent to the Controller driver as: HANDLE\_SET\_INPUT, HANDLE\_ DISCONNECT\_INPUT and HANDLE\_ SET\_VOLUME.

The controller driver handles these commands through the standard ExecuteCommand function. Note that the driver ignores any of the native proxy commands typically received through ReceivedFrom Proxy.

Non-binding keys used:

- ACME\_MOIP\_TRANSMITTER\_BUS | Consumer
- ACME\_MOIP\_TRANSCEIVER\_BUS | Consumer
- ACME\_MOIP\_TRANSCEIVER\_BUS | Provider
- ACME\_MOIP\_RECEIVER\_BUS | Provider


**acme\_distributed\_media\_transmitter.c4z**

The example transmitter driver delivers AV changes using the commands: SET\_INPUT, DISCONNECT\_INPUT and SET\_VOLUME. These are sent to the controller driver as: HANDLE\_SET\_INPUT, HANDLE\_ DISCONNECT\_INPUT and HANDLE\_ SET\_VOLUME.

Non-binding keys used:
-   
	ACME\_MOIP\_TRANSMITTER\_BUS | Provider


**acme\_distributed\_media\_receiver.c4z**

The example receiver driver handles incoming AV changes using the commands: SET\_INPUT, DISCONNECT\_INPUT and SET\_VOLUME. These are sent to the controller driver as: HANDLE\_SET\_INPUT, HANDLE\_ DISCONNECT\_INPUT and HANDLE\_ SET\_VOLUME.

Non-binding keys used:

- ACME\_MOIP\_RECEIVER\_BUS | Consumer


**acme\_distributed\_media\_transceiver.c4z**

The example transceiver driver has the ability to deliver and receive AV Commands: SET\_INPUT, DISCONNECT\_INPUT and SET\_VOLUME. These are sent to the controller driver as: HANDLE\_SET\_INPUT, HANDLE\_ DISCONNECT\_INPUT and HANDLE\_ SET\_VOLUME.

Non-binding keys used:

- ACME\_MOIP\_TRANSCEIVER\_BUS | Consumer
- ACME\_MOIP\_TRANSCEIVER\_BUS | Provider

It is highly recommended that you exercise the sample drivers in a project, bound to source and endpoint device drivers. Studying the debug output in the drivers (particularly the Controller driver) will help clarify the concepts presented here. This exercise, in conjunction with the driver documentation included, will help you  best model AV non-binding connections in your drivers.


[1]:	https://github.com/snap-one/docs-driverworks/tree/master/sample_drivers/nonbinding_connections