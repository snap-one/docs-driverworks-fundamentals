## What was New in O.S.3

### Sample Drivers

Several new sample.c4z files have been included in this release of the SDK. These drivers can be located in the SDK under the root level or: DriverWorks SDK/Samples: They include:

- Variable Parser driver which provides the ability to convert any variable on any device from one type to another. See the driver documentation for more information.

- The dlna example driver demonstrates how to do DLNA discovery and control.\_ 
- The generic-http driver demonstrates how to perform basic C4:url calls.

- The dynamic list widget driver demonstrates the ability to generate Dynamic Lists based on driver code to support a proxy-less device.

- The notification driver demonstrates how to implement push notifications of images in a driver.


### Other:

New information regarding the use of the CUSTOM SELECT Property has been added.

Lua Conditional functionality has been enhanced for the number type. Two new options include the \<minimum\> and \<maximum\> tags to specify the range for the NUMBER type

An overview, usage section and examples on creating Universal Minidrivers has been added.

New information regarding persisting driver data has been added.

New information regarding limitations with signed bit integers in excess of 32 bits has been added to the Handling Binary Data section.

A new connection type has been added: Non-Binding Connection.

Beginning with OS 3, driver developers can leverage Lua's Just In Time compiler (LuaJIT).

New information regarding driver debug messaging and LuaJIT updates has been added to the Driver Update Considerations area.

The Device and Service Icon presentation content has been is updated in conjunction with OS 3. This includes new icon guidelines, design templates and instructions to assist with delivering the best experience across a variety of wallpapers and to ensure consistency across all icons within the interface. The new content can be found in the DriverWorks SDK at:
DriverWorks SDK/Icon Templates directory.