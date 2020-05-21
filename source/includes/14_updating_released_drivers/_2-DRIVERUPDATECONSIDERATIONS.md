## Driver Update Limitations and Considerations

!Driver developers may occasionally need to make a change to an existing driver to provide additional functionality or correct a defect. When considering this, it is important to be aware of what is and what is not permissible.This will assist with making the best decision regarding whether or not a driver can or should be updated versus a new driver being created and released. 

The benefit of updating a driver is that existing users of that driver can use the Driver Update functionality within Composer to get access to and take advantage of the changes made. This is much easier than asking them to remove the old driver, add the new driver, and reconfigure it. With a driver update, all configuration remains intact.
 
**Hardware/Protocol Changes** – If the driver is for a device that is being updated with new hardware and/or a new control protocol, the driver developer should almost always create a new driver with a distinct driver name and filename. This will allow the existing users of the previous hardware/protocol to continue to use the previous driver. New users can install the new version of the driver.
 
**Defect Resolution/New Functionality** – When resolving defects or providing new functionality that is limited to  to Lua code: driver.lua or an included lua library file - an update to the the existing driver is the best practice. Please make sure that you increment the \<version\> and change the modification date so that users can distinguish between the drivers. You may also want to consider including a changelog in your driver documentation that outlines the changes made.
 
**New Functionality and New Proxy Instances** – If the device, or the system you're connecting to features new functionality that you want to expose by adding a new \<proxy\> declaration within the driver, you MUST create a new driver. Each proxy instance declared by the driver causes director to load a corresponding proxy driver when your driver is added to the project. The only time these proxy-protocol drivers are added and dependent internal bindings made is when the driver is added to the project. So, if you create an updated driver with a new proxy instance, and an existing driver is updated to your new version, it will be lacking the dependent proxy driver and internal binding to it. This causes project corruption. The same thing applies to removing a proxy instance or changing the type of proxy used by your driver. If you must make any of those changes, you must create a new driver.
 
**Modeling Changes in `<connections>`** - Generally, it's not a good idea to change the connections declarations with a driver update. However, if you must do so, you may only add. You should not remove connections, or change the type or definition of your connections in driver.xml. This can cause the project configuration to become broken for that driver when it is update.
 
**Other Changes to driver.xml** – Consider carefully before making changes to any of the following in driver.xml or in the corresponding DriverWorks Lua interfaces:

- Properties – Generally, the changes you make to properties are safe across driver update. You may add and remove; but be careful about retyping properties.

- Events – Dealers may program custom functionality to be executed with an event is triggered. You won’t want to remove any events (or change the Event ID) or it may cause problems with existing programming.

- Variables – Dealers may evaluate variables or set variable values in custom programming. Adding variables will not cause any problems. Removing variables or renaming, re-typing, or changing ID may cause problems in an updated driver.

- Commands – Dealers use commands to create custom functionality to be executed with an event is triggered. You won't want to remove any commands or it may cause problems with existing programming.

**Driver Debug Messaging Considerations and LuaJIT**
Beginning with OS 3, the ability to send debug Lua messages upon a driver failing to update has changed. Sending Lua debug messages are determined on how the driver's properties are set. When a driver is updated from using the previous PUC Lua compiler to LuaJIT, the driver's state and properties are temporarily lost. This is also true if a driver is updated involves going from LuaJIT to PUC Lua. The state and properties are not restored until the driver has loaded successfully. During this time frame, driver debug messages are not sent. The following are use cases to consider regarding this:

**Driver updating from PUC to PUC.**
This is a driver that was initially loaded with PUC and the updated driver likewise loads with PUC. There is no "jit" attribute applied to the driver script. In this case, the same driver instance is reused and the driver will function as it did prior to OS 3.

**Driver updating from PUC to LuaJIT:**
This is a driver that was initially loaded with PUC and the updated driver expects to load with LuaJIT. The specified "jit" attribute has been applied to the driver script. In this case, the old driver instance is destroyed and then re-loaded with LuaJIT. In doing so, all of the driver properties that set for this driver are temporarily lost. This results in the following two scenarios:
If the driver loads successfully all of the properties & state data is restored.
If the driver fails to load successfully, the LuaJIT instance is abandoned and then reloaded with PUC Lua. The properties & state data is restored after the driver loads. Debug Lua messaging is  impacted.

**Driver updating from LuaJIT to LuaJIT:**
This is a driver that was initially loaded with LuaJIT and the updated driver likewise loads with LuaJIT. The specified "jit" attribute has been applied to the driver script. In this case, an attempt is made to re-use the same driver instance. This results in the following two scenarios:
If the driver loads successfully, then this will function as it did prior to OS 3.
If the driver fails to load, the LuaJIT instance is abandoned and reloaded with PUC. When this happens, all of the properties that are set for the driver are temporarily lost. The properties & state data is restored after the driver loads. Debug Lua messaging is impacted.