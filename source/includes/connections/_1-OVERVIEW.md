## Overview

As every trained Control4 installer knows, each driver within the Control4 system publishes the connections capabilities that are available for the device being controlled. These connections are utilized to create the bindings within the Control4 system. The connections to the device you are creating a driver for can be implemented using serial (RS-232) or Internet (TCP/IP) protocols.
Devices with serial ports are bound using the Control/AV tab to bind the Control Inputs to the appropriate serial port on any controller or serial extender in the Control4 system

Devices communicating using Internet protocols are configured using the Network tab to identify the IP address of the device. 
In addition to the network/communication bindings, your DriverWorks driver will have all of the other bindings necessary for your device (just like any other driver) as long as they are correctly defined in the \<connections\> section of the .c4z file.
As the driver creator, you will define those connections capabilities within the .c4z file.
Connections are critical and can be complicated. The following information outlines the various parameters that can be utilized in the .c4z file for configuring connections.

`<connections>`
This section contains zero or more connections. Connections are the bindings defined by this driver and usually visible through Composer. The AV bindings are reported to the proxy driver when they are bound together.

`<connection proxybindingid=”3001”>`
This establishes a connection block. The proxy binding id attribute: 3001 allows the driver to specify which proxy binding that this connection is associated with.

`<id>5001</id>`
This is the id for the connection. WARNING! This id number must be in the correct range for the particular type of connection. Below are the ranges:

| Binding | Range |
| --- | --- |
| Control Bindings | 1 - 999 |
| Video Inputs | 1000 - 1099 |
| Video Outputs |	2000 - 2999 |
| Audio Inputs | 3000 - 3099 |
| Audio Outputs |	4000 - 4999 |
| Proxy Bindings | 5000 - 5999 |
| Network Bindings | 6000 - 6999 |
| Room Bindings | 7000 - 7999 |
| Power Manager | 8000 -\> 8999 |


`<facing>0</facing>`
This value indicates which side of the device this connection is on if it is a physical connection. Values are:

Front - 0
Back - 1
Top - 2
Bottom - 3
Left - 4
Right - 5
Unknown - 6


`<connectionname>TV</connectionname>`
The value indicates the name of this connection. This name is used by Composer to display the binding if it isn’t a hidden binding.


`<type>2</type>`
The value indicates the type of this connection. Values are:
Control - 1
Proxy - 2
Audio/Video - 3
Network - 4
Video - 5
Audio - 6
Room - 7


`<consumer>False</consumer>`
This value (True or False) indicates if this driver is a consumer (or a provider) of this connection.


`<linelevel>False</linelevel>`
This value (True or False) indicates if this connection provides a line level source.


`<idautobind>0</idautobind>`
This is an optional tag that allows the connection to automatically connect to a specified binding ID Range is 0 - 999.


`<hidden>True</hidden>`
This is an optional tag to specify if this connection should be hidden or not. This defaults to false, except for proxy bindings.


`<classes>`
Section of one or more classes for this connection:

`<ports>`
   `<port>`
`<number>8750</number>`
   `</port>`
`</ports>`

If the class is TCP or UDP then there may be a ports section as seen above. This includes the IP Port number for the network connection.


`<auto_connect>True</auto_connect>`
The above example creates an Auto Connect for this socket. This socket will be created and connected when the driver is started.


`<monitor_connection>True</monitor_connection>`
The above example provides monitoring of this socket. Director will periodically check the connection, if data is not returned on this socket it will be considered down.


`<keep_connection>True</keep_connection>`
The statement above keeps this connection connected. This means if the connection goes down, Director will attempt to re-connect it.


`<delimiter>0d</delimiter> `
Above is an example of an optional delimiter to specify how the network driver should break up incoming packets   NOTE: If delimiter is specified, data will not be sent UNTIL delimiter is reached.  If no delimiter specified, data is sent on timeout.


`<grow>255</grow>`
This is the number of bytes to grow the size of receive buffers by when the original receiver buffer is overflowed.


`<autobind>True<autobind> `
The optional flag above marks this class as needed to be autobound. If an idautobind is provided then this class will be connected on that binding, otherwise it will be connected on the first connection in the project providing this class.


`</connections>
`The above closes this connections section.
