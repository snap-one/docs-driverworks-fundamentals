## Overview

Universal Minidrivers are small, self-contained Media Player proxy drivers designed to provide a UI-selectable driver for an individual media service, such as Netflix.  Minidrivers are designed to be lightweight with minimal control capability of their own.  Any UI interaction with a minidrivers will, by design, forward the interaction on to whichever device driver is providing the service audio and video, such as a Roku player.

Core Concepts
Each minidriver is device-ambivalent.  A single minidriver can be connected to as many or as few device drivers in the project as required of any supported type.  This eliminates any need for polluting the project and UI with “Roku Living Room Netflix”, “Roku Master Bedroom Netflix”, “Amazon Fire Netflix” drivers.  It also means that the Composer online database will only need to contain a single Netflix driver, rather than one for every device that supports Netflix.

The minidriver concept only provides for controlling a device providing a UI.  It does not provide for any ability to browse the selected service or provide feedback on what is currently playing on any one device.

The associated switcher logic (to be added to the device driver implementing universal minidriver support) has optional components to “take over” as the room selected device once the minidriver is selected.  This is more useful when the device itself implements the media player proxy than when the device is a receiver or TV, simply because the UI presented in Navigator for a media player provides more useful buttons for control of a media service than the TV or Receiver proxies do.  This is toggled in the device driver (not the minidriver) using the “Passthrough Mode” property, where setting “Passthrough Mode” to “On” will leave the minidriver selected in the room, and the minidriver will pass through any proxy commands to the switcher driver.


### Minidriver Specifics
A Minidriver.c4z file contains a common core in `common/universal_app.lua` with a very small amount of service specific code (in driver.lua).  The common core code should not be changed under any circumstances by non-Control4 developers.

The driver.lua code defines two variables:

`APP_NAME` This is the full name of the service.

`SERVICE_IDS` This is a table which contains keys of the unique device type code used by a device driver (or class of device drivers) mapped to a code that defines how to launch this service on that device.  An example from the Amazon Video minidriver is to the right.

```lua
SERVICE_IDS = {
    UM_ROKU = '13',
    UM_SAMSUNG2017 = 'amazon',
    UM_AMAZON = 'com.amazon.avod',
    UM_SONY_TV = 'Amazon Video',
}
```

The channel ID for Amazon Video on Roku is 13.  The application name used to start the Amazon Video service on the Amazon Fire products is com.amazon.avod.  These are the values that will be discovered and used by the device specific drivers for their application launch code – so the Roku driver will discover the channel ID of 13 and plug that into the channel launch function unique to Roku, ignoring the other values from this table.

These variables are used in the `common/universal_app.lua` code to generate variables. The switcher component of the schema talks to these variables. They are also used in the internal build process to generate the documentation and driver.XML for each Minidriver must conform to this format.


### Making a New Service Driver
A new service, “Play Net Movies”, has been launched and is available on Roku devices only at launch.  Three items are required to create a Minidriver for this new service:

- The name of the service to populate the value of `APP_NAME`
- The channel ID on Roku to populate the value of the `UM_ROKU` key in the `SERVICE_IDS` table. If this is available on other devices, the unique id for those devices as well
- Logo artwork for the service, in 16x16, 32x32, 70x70, 90x90 and 300x300 pixel images.

The artwork should be placed into the www/icons folder with these names:

- 16x16 – `device_sm.png`
- 32x32 – `device_lg.png`
- 70x70 - `/device/experience_70.png`
- 90x90 - `/device/experience_90.png`
- 300x300 - `/device/experience_300.png`

The driver should then be run through the build process at Control4 by submitting this information to \<universal\_minidriver\_dev@control4.com\> for inclusion in the online driver database.

Alternatively, it's possible to take an existing driver and edit the driver.lua and driver.xml to have the correct values.  If you do this, please also still submit the information to \<universal\_minidriver\_dev@control4.com\> so we can update the online driver database.


Making a New Device Driver Compatible with Universal Minidrivers
New media playback devices are coming online all the time and existing ones continuously receive additional functionality.  Therefore, it may become necessary to add the capability to select Universal Minidrivers to a driver that is either under development or already released.

To that end, we have included in this package components of code that should be included in the driver Lua and XML.  They are found in the switcher directory. These files are marked up with comments that should help when implementing Universal Minidriver support. Below are some additional instructions for clarity.


### XML
The driver.XML defines several sections of XML that should be copied into your driver.XML.

The \<properties\> section defines one property.  If your driver already has a \<properties\> section, copy the single property into your properties section. Do not create a new \<properties\> section.

The \<proxies\> section defines one additional avswitch proxy that may need to be added to your driver.

If your driver is implemented using the TV, Receiver, AV Switch or other proxy that handles input selection using `SET_INPUT` you do NOT need to include this proxy. You MUST change all the proxybindingid values of the `RF_MINI_APP` connections to this proxy ID.

If your driver is implemented using the media player or other proxy that does not handle `SET_INPUT`; you MUST include this proxy and MAY change the proxy ID here.  Note that updating an existing driver in a project with a driver that has an additional proxy will cause project corruption. Developers updating an existing driver must be careful to not cause this project corruption by changes within their driver.

The `<connections>` section defines AVSWITCH, App Switch Video Out and App Switch Audio Out connections. These are only needed if you included the avswitch additional proxy.

The remaining connections are of class `RF_MINI_APP`.  This is a special class created for Universal Minidrivers and should not be changed unless you did not need to include the additional proxy or you changed the proxy ID of that additional proxy. If this is the case, you must change the proxybindingid value to match the correct proxy.

By default, the `RF_MINI_APP` connections start at 3101 and go through 3125.  This can be changed if needed or desired. However, they must be sequential. Please make sure to adjust the `MINIAPP_BINDING_START` and `MINIAPP_BINDING_EN`D values in the OnDriverInit section of the Lua to match these changes.


### Lua
In the Lua driver, there are several functions marked as "MERGE" and several marked as "COPY".
The functions marked as MERGE are all entry points to your driver defined in DriverWorks and these may already exist as functions in your driver.  Please carefully merge the contents of these functions into your existing code without changing any of the statements themselves.

The functions marked as COPY are new functions not defined in DriverWorks. Verify that you do not already have functions with these names. If not, these are safe to simply copy as-is into your code. 

There are some initialization values in the OnDriverInit section that should be confirmed:

`PASSTHROUGH_PROXY`
This is the Proxy ID of the proxy in the switcher driver that will receive any proxy commands from the Minidriver such as transport controls, cursor controls , etc.

`SWITCHER_PROXY`
This is the Proxy ID of the proxy in the switcher driver that has the `RF_MINI_APP` connections bound to it

`USES_DEDICATED_SWITCHER `
Used to hide the `SWITCHER_PROXY` proxy from all UIs

`MINIAPP_BINDING_START`
First connection ID of the `RF_MINI_APP` connections.

`MINIAPP_BINDING_END` 
Last connection ID of the `RF_MINI_APP` connections

`MINIAPP_TYPE`
This is the Unique ID used to key into the `SERVICE_IDS` table in the Minidriver.  It is how this driver retrieves the unique code required to launch the selected service. 


### Finishing Up
In order to test that your driver supports Universal Minidrivers you will likely need to modify an existing Minidriver to support your new device as described above.  Once you are confident that the driver is working, but BEFORE releasing your driver, please contact ,universal\_minidriver\_dev@control4.com. to confirm the `SERVICE_IDS` table entry you have chosen is unique and descriptive. You will also need to provide a list of values for all services that your device supports so they can be included in the online database in time for your release.


**For sample drivers please see the Samples/Universal Minidrivers area of the DriverWorks SDK.**