## DriverWorks and Zigbee Functionality


DriverWorks support for Zigbee functionality was originally delivered in release 1.6.1. This section has been included to provide information to Control 4 partners already using the Zigbee SDK. This information will support the development of DriverWorks Zigbee functionality so that partners using the Zigbee SDK can integrate into the Control 4 system.

Functionality provided in support of Zigbee functionality currently includes:

- Allowing DriverWorks driver developers to send data to and receive data from ZigBee devices using either EmberNet or ZigBee Pro

- Allow ZigBee SDK developers the ability to update their ZigBee devices using DriverWorks.

- Allow ZigBee SDK developers the ability to utilize the Control4 identification mechanism, but define their own ID strings.

For detailed information regarding supported Zigbee specific APIs, please see the [Zigbee Interface][1] documentation.

### Updating Zigbee Device firmware Using DriverWorks

_Note: The following is applicable to operating systems 3.3.1 and earlier. 3.3.2 and later support the use of [over the air device updates][2]._

The ability to use DriverWorks drivers to update firmware on ZigBee devices has been implemented using two features:

1. Support of encoded BLOB data (firmware) within the driver file
2. Functions to handle device requests and delivery of the firmware data.

**Encoded BLOB Data in the Driver File **

DriverWorks supports the ability to store binary firmware data within the driver file. This data must be Base64 encoded. The encoded firmware data string information should reside at the beginning of the driver file as seen in the example to the right:


```xml
<devicedata>
 <copyright>Copyright 2019 Control4 Corporation. All rights reserved.</copyright>
   <largeblobs>
    <largeblob name="Blob1">VGhpcyBpcyBOaGUgRmlyc3QgQkxPQiENCg==</largeblob>
   <largeblobname="Blob2">AAECAwQFBgclCQoLDA0ODwABAgMEBQYHCAkKCwwNDg8=</largeblob>
  </largeblobs>
 <config>
```


When ZigBee devices come on-line they can validate firmware versions with Director. If an update is available, the data is sent accordingly to the device. Drivers that wish to update their device’s firmware should do it in a sequential manner. The functions delivered in this SDK support this. It is possible to place the BLOB data in the Lua section of the driver. However, this requires a copy of the firmware data within every loaded driver used by the device. For example, if 15 devices use the driver, it will be necessary to maintain 15 copies of the firmware update data loaded in memory.


**Firmware Request Functions**

When a device requests a firmware update (the version of the firmware on the device is older than the firmware the driver has), the driver should request a 'reflash lock' via [RequestRefreshLock][3]. This allows the driver to ensure that it is the only driver updating a device at any time. This is required so that Director can manage multiple Zigbee drivers so that only one driver will be allowed to update at a time.  This ensures that device firmware updates don't overload the zigbee network.

The [OnReflashLockGranted][4] function is called by Director when the driver has permission to update the firmware on the device.  This function ensures that the driver is the only device using the zigbee network to flash a device. This ensures that the delivery of the firmware update won’t be slowed down due to other firmware updates from other devices.

All reflash traffic is sent to the device using standard [SendZigbeePacket][5] and [OnZigbeePacketIn][6] commands/functions as described above.

[1]:	https://snap-one.github.io/docs-driverworks-api/#zigbee-interface
[2]:	https://snap-one.github.io/docs-driverworks-api/#zigbee-interface-zigbee-ota-device-updates
[3]:	https://snap-one.github.io/docs-driverworks-api/#zigbee-interface-requestreflashlock
[4]:	https://snap-one.github.io/docs-driverworks-api/#zigbee-interface-onreflashlockgranted
[5]:	https://snap-one.github.io/docs-driverworks-api/#zigbee-interface-sendzigbeepacket
[6]:	https://snap-one.github.io/docs-driverworks-api/#zigbee-interface-onzigbeepacketin