## Automatically Updating Released Drivers

The ability for a driver to update to a newer version through a new ComposerPro interface has been added in conjunction with O.S release 2.9.0. The Automatic Driver Update functionality only applies to drivers loaded into Control4’s online driver database. This is similar to the automatic application update capabilities provided by the Apple iTunes store and Google Play store for example. If you are a 3rd-party Developer and would like to utilize the automatic driver update functionality, you may work with Control4 and participate in the driver certification program to get your drivers posted on the Control4 driver database.

For consideration regarding whether or not your driver is a candidate for Driver Update (as opposed to a new driver release) please see Driver Update Considerations

There are two main components involved in supporting this functionality. These include several, new XML tags that need to be included in the driver. These new tags can reside anywhere at the top level of the driver XML under `<devicedata>`. The second component involves enabling the Driver Update Agent in ComposerPro.

The new driver XML tags include: 

`<auto_update></auto_update>`
This tag designates if a driver supports automatically updating to newer version when set to true. For example:

`<auto_update>true</auto_update>`


`<minimum_auto_update_version></minimum_auto_update_version>`
Assuming `<auto_update>` is set to true, this tag designated the minimum required driver version before an update can be applied automatically.  For example: 

`<minimum_auto_update_version>4</minimum_auto_update_version>`

The above example indicates that the driver found in the project must be version 4 (at a minimum) in order for the driver to be updated automatically.

`<minimum_os_version></minimum_os_version>`

Assuming `<auto_update>` is set to true, this tag designates the minimum required Control4 Operating System before an update can be applied automatically.  For example: 

`<minimum_os_version>2.8.0</minimum_os_version>`

The above example indicates that the Control4 operating system being used must be version 2.8.0 (at a minimum) in order for the driver to be updated automatically.

`<force_auto_update>false</force_auto_update>`

The force auto update tag, when set to true, ignores the Enable Driver Update flag set by dealers in ComposerPro _see below_. It will update on the scheduled time regardless of the setting of the flag.

As mentioned above, the other component to managing driver auto updates is found in ComposerPro's Manage Drivers/Auto Update screen. The screen is accessed through the ComposerPro toolbar by selecting Driver/ Manage Drivers:

<img src="images/14_1-01.png"/>

Note the addition of the new check box called Enable Driver Update Agent:

<img src="images/14_1-02.png"/>

When the check box populated, every driver that has `<auto_update>true</auto_update>` will be listed here. The check boxes next to the driver’s name are used to keep that specific driver from auto updating even if the `auto_update` XML tag is set to true. Leaving the Enable Driver Update Agent check box unpopulated will prevent any drivers from auto updating – regardless of XML settings.

