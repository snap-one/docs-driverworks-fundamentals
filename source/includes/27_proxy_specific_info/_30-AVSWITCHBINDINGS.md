## AV Switch Proxy and Display Adjustable Bindings

O.S. releases 3.3.0 and later provides the ability to hide AV Switch Bindings that are not in use. This feature has been provided to better manage the UI display in ComposerPro. This is especially useful for AV Switch devices that have a large number of bindings. Displaying all of the bindings for these devices can make the use of UI cumbersome.


**Assumptions:**

- All applicable bindings are correctly defined in the driver’s XML.
- Driver developers already have the ability to create control bindings using [C4:AddDynamicBinding()][1]
- The driver file specifies bindings to be hidden by the proxy through the use of a new XML element delivered in O.S. 3.3.0:  `<ExtraInfo>D2</ExtraInfo>` _See below for more information._
- Resizing of the bindings list is possible only if a new capability delivered in O.S. 3.3.0 is set to true: `<allow_dynamic_resize>`  _See below for more information._


**AV Switch Proxy Functionality Using Display Adjustable Bindings**

- Bindings that are bound and then unbound in ComposerPro will only be hidden after a Director restart.
- The Proxy will increase the amount of displayed bindings based on the last unhidden, bound binding. For example,  bindings 1 through 10 are all bound and unhidden. ComposerPro will display all of those bindings and in addition, binding 11 through 13 assuming they are also unhidden.
- The largest Binding amount will be shown ACROSS ALL SECTIONS at the same time.  For example, RX, TX, Audio, Video will all be expanded to the largest bound set of any of the section. If RX has 10 bound connections that means Audio will also display 10, regardless of binding status.
- The driver developer is responsible for handling the configuration use case of virtual director or when the device is not available and ensuring that the solution is documented and appropriate for these scenarios.


**Driver XML to Support Hidden Bindings**

To allow the AV Switch Proxy to auto hide/unhide a binding, the driver needs to have a new XML tag included in the each connection in the driver’s XML: `<ExtraInfo>D2</ExtraInfo>`.

The ExtraInfo tag must include the value of `D2` as the parameter for each connection. For example, see the XML for connection 3096 to the right.

```xml
<connection>
     <id>3096</id>
     <type>6</type>
     <ExtraInfo>D2</ExtraInfo>
     <connectionname>TX 96</connectionname>
     <consumer>True</consumer>
</connection>
```



** Capability to Support Hidden Bindings**

**allow\_dynamic\_resize**

Capability that allows for AV Switch bindings to be hidden in ComposerPro. Defaults to false.

** Signature**

`<allow_dynamic_resize>false</allow_dynamic_resize>`



**Additional Information and Expected Limitations**

- The driver developer is responsible for persisting and recreating any non-binding connections when director starts up.
- The driver developer must recreate the bindings in the proper order before director loads and creates the connection. This is done by the driver developer by persisting the binding information and creating the bindings on execution of the main driver code.
- The driver must handle the "virtual" or device not available case appropriately based on the expected use case for configuring the device. If the connections aren't defined in xml, and the device isn't available, the installer may have a limited experience. The driver developer needs to take this into consideration.


For additional Information, please see the DriverWorks Proxy and Protocol Guide regarding:

[Navigator EQ AV Switch Proxy Commands][2]

[Navigator EQ AV Switch Protocol Notifications ][3]

[Navigator EQ AV Switch Capabilities][4]

[1]:	https://snap-one.github.io/docs-driverworks-api/#miscellaneous-interface-adddynamicbinding
[2]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#amplifier-navigator-eq-navigator-eq-proxy-commands
[3]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#amplifier-navigator-eq-navigator-eq-protocol-notifications
[4]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#amplifier-navigator-eq-navigator-eq-capabilities