## Drivers and Communication

Effective communication between your driver and Control4 System is paramount to a successful end user experience. In this section we’ll look at a few of the supported communication methods and discuss several Control4 APIs delivered in the DriverWorks SDK that facilitate communication between the driver and the Control4 O.S.

In keeping with our Receiver example, the act of turning the receiver on seems rather insignificant to the end user. The remote is picked up and the on is pushed. Pretty straightforward. But how does this model apply to a controlled environment where we’re not using the provided Receiver remote and our receiver needs to reside in a Control4 System?

When our on is pushed, an infrared signal is transmitted from the remote to the Receiver. This signal is turned on and off in a particular pattern. The frequency or pattern of the on or off represents a code. When the device receives and decodes the signal – it performs a “On” action.

A DriverWorks driver written for this particular receiver needs to contain the IR codes needed to control it. Your job as a driver developer will be to obtain these IR codes for the device your driver will control. When a user picks up a Control4 remote or navigates to the receiver and selects it, it executes the appropriate IR Code associated with On – from within the driver.

The DriverWorks SDK includes rich set of APIs. Here is our SendIR API documentation.


## SendIR

Function called from DriverWorks driver to send an IR Code. This API should not be invoked during OnDriverInit. 

### Signature

`C4:SendIR(idBinding,idIRCode)` 

Parameter | Description
| --- | --- |
| idBinding | IR Binding ID to send the IR Code. |
| idIRCode |  ID of the IR Code to send from .c4i/.c4z |
| idBinding | Proxy Binding ID. (optional) |


### Returns

`None`


### Usage Note
The IR code to send must be declared as an \<ircode\> in the \<irsection\> of the driver’s .c4i/.c4z file.

Based on the information here, we know that in order to use the API we need to send a control connection ID value for the device we’re controlling as well as the ID for the IR Command we wish to send.
Our IR Code is sent to Director using the SendIR API. Director then responds by executing the code and controlling our device. Your driver’s job is to send the correct command using the appropriate settings to achieve device control.

Let's take a look at another means of device communication supported by Control4 – Serial, also known as, RS 232.  Serial control offers driver developers a feature that IR does not – it supports two-way communication. When we refer to “two way communication” we are referring the ability to send a commands to a device AND receive status and feedback from that device. This is not insignificant, especially when you consider the benefits of being able to ascertain a device’s state before you send it a command.

This is also a good time to look at some Serial control API’s that are available through the DriverWorks API Reference Guide. Remember when we mentioned that serial control has a significant benefit as it provides the ability to receive feedback from a device? That feedback comes through on an API called: ReceivedfromSerial. This API requires the control connection ID through which the data was received through along with the actual transmitted data from the device.

## ReceivedFromSerial

Function which dumps the data received from serial (hex format) for inspection via print.  It then evaluates the response for specific delimiters and extracts the necessary components which are then used to do something. This API should not be invoked during OnDriverInit. 


### Signature

 `ReceivedFromSerial(idBinding, strData) `


Parameter | Description
| --- |  --- |
| idBinding | Binding ID of the serial interface the data was received on. |
| strData | Data received from the serial interface Returns None. |


### Usage Note:
Serial data received may contain NULL characters. NULL (‘0’) is a valid character and Lua strings handle embedded NULL characters. Serial data received may only include part of a protocol command from the connected device. It is the driver’s responsibility to re-constitute and parse the commands received from the device.


SendToSerial is another API worth mentioning. It provides a means to send data to your device:

## SendToSerial

Simple function which sends the command out serial port on binding 1 and adds the r terminator to the end of the command being sent. This API should not be invoked during OnDriverInit.


### Signature  

`C4:SendToSerial(idBinding, strData)`


Parameter | Description
| --- | --- |
| idBinding | Binding ID of the serial interface to send on. | 
| strData | Data to send out the specified serial interface  |


### Returns

`None`


### Usage Note
Serial data to be sent can contain NULL characters. NULL (‘0’) is a valid character and Lua strings handle embedded NULL characters.

Like ReceivedfromSerial, it requires a binding ID and the data string. Using these two APIs we can send commands to our receiver from a remote or a touch panel and then receive information back that indicates status or provide us with the ability to enact further control measures.