## Driver Development Terminology

**Actions** - Actions are similar to device specific commands. However, they are primarily intended to be used during driver installation and configuration. They can only be activated on the Actionstab in Composer and are not available for programming within the system. 

**Android Navigator** – A graphical interface displayed on devices running the Android OS platform.

**Capabilities** - Capabilities are defined in a proxy and referenced in protocol drivers. They represent typical functionality and physical characteristics found in devices of a given device class. Values for each Capability are entered in a protocol driver and then in-turn are exposed to the Proxy.  This enables the proxy driver to determine what capabilities are supported by the device defined by the protocol.

**Combo Driver** – A standalone driver that does not bind itself to a Control4 proxy. User interaction is accomplished through programming, events and bindings. A Navigator UI is not provided for combo drivers. The `<combo>true</combo>` tag must be added to every combo driver. This addition of this tag and its true setting in a combo driver (drivers that contain their own proxy and protocol) will avoid naming conflicts. Without the tag, the combo driver and its proxy must use identical names.

**Command** – A command comes from the Control4 system and its destination is a device or a driver.

**Config** - The config area of a protocol driver contains all of the device specific configuration properties.  The Lua code is also incorporated into the Config section.

**Connections** - Connections are the bindings defined in a driver and used within the Control4 system. Generally speaking, they are visible through Composer. In the case of AV, Control and Room bindings – they are reported to the proxy driver when they are bound together in a project.

**Connection Class** - Connection class information is defined inside each connection instance – within the connections code block of a driver. A connection class lists the physical data or signal type that a specific connection can to transmit.

**Director** - Director is the software platform that runs on all Control4 controllers. It communicates with device drivers in a manner that enforces a standardized API which hides the complexity of a huge array of different systems and protocols. It also uses that communication to serve up various user interfaces that are displayed on a wide variety of navigation devices.

**Driver** – A term used to describe files used in the Control Operating System. They are text-based files that have an extension of .c4i or .c4z. Several types of drivers exist including proxy drivers, protocol drivers, combo drivers and multi-proxy drivers.

**iOS Navigator** - A graphical interface displayed on devices running the iOS platform.

**List Navigator** – A text based Navigator instance that is used on Control4 Remote controls.

**Multi-Proxy Driver** – A Protocol driver that has dependencies upon several proxies. A receiver driver is good example of this. While the protocol driver for a Sony receiver is certainly device specific – it relies upon several proxies such Tuner and DVD in addition to the Receiver proxy.

**Navigator** – Refers to an instance of the Control4 UI running on a device.

**Navigator Device** – Refers to any device that is running a version of the Control4 Navigator. This includes TVs (onscreen navigator), touchscreens, remote controls, etc.

**Navigator UI** – A user interface that is graphically displayed on devices or may also refer to a text-based user interface as in List Navigator on remote controls.

**Notification** – Notifications are updates sent to the Control4 system that communicate status or update changes. These notifications may come from a device or the driver.

**Programming UI** – A user interface that does not utilize a graphical or text based user interface. This is usually implemented through programming, button presses, timers or events.

**Properties** - DriverWorks Properties, as defined in the .c4i file, are exposed in the Composer System Design interface on the Properties tab. Properties are data values which are used within your driver. These are initialized in the driver code and also may be configured through the Composer Properties page for your driver. Property data types include:

**Protocol **– Protocols contain the definition for a specific device. They are comprised of functionality that is unique to a specific device.

**Protocol Driver** – An implementation of the protocol in the form of a driver (.c4i) file. When this driver is added to a Control4 Automation system, device control is dictated by the functions defined in the protocol by the device manufacturer. User Interaction with the device is accomplished through system programming such as button presses and events. The UI is provided by one or possibly several of the proxies. In most instances of driver development, the driver developer will be creating a protocol driver.

**Proxy** – A Proxy contains the definition for a specific class of device. Proxies are comprised of the most common examples of functionality for each device class. The use of a proxy provides a common interface between the Control4 Navigator UI and a device driver. Proxies are pre-defined and supplied by Control4.

**Proxy Binding Id** - A proxy binding id is a numerical value that is assigned to each proxy used within a protocol driver. It is a unique reference that can be utilized in the protocol driver code. This id value is used often when sending data and ensures that the correct proxy-data relationship is always enforced.

**Proxy Driver **– An implementation of a proxy in the form of a driver (.c4i) file. When this file is included in a Control4 Automation system, device control includes that of the pre-defined functionality provided in the proxy. User interaction is provided through a Control4 Navigator interface.

**UI** – User Interface. General term referring to the manner in which a user interfaces with the Control4 system.