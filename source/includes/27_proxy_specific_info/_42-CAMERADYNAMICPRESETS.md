
## Camera Proxy and Presets

_Note: the following content is only applicable to Composer Pro 4.0.0 and later. This functionality has been available in Navigators prior to Composer Pro 4.0.0_

Support has been included for camera drivers that have the ability to deliver dynamically created presets. When the [has\_dynamic\_presets][1] capability is set to True, the following screen appears in Composer pro:

<img src="images/camimage1.png"/>

**New** - The New button allows for a new camera preset to be set and saved. The camera needs to be navigated to the desired position before clicking the New button.

**Remove** - Removes an existing preset.

**Select** - The select button moves the camera to a selected preset from the preset list.

**Rename** - The Rename button allows for an existing preset to be renamed.

**Replace** - The Replace button allows the camera to be repositioned for an existing preset. The original preset name is maintained. The camera needs to be navigated to the desired position before clicking the Replace button.

Dynamic Presets are also available for Composer pro programming purposes. Once configured, they are available under the driver’s Actions area of Programming in the Presets drop-down box.


_Note: the following content is only applicable to Composer Pro versions prior to 4.0.0._

Original support for camera drivers that do not support dynamic presets involves a numbered set of presets being displayed in the Composer Pro UI. The following screen is displayed in Composer Pro when the [number\_presets ][2]capability is set to the amount of desired presets:

<img src="images/camimage2.png"/>

In the example above, the number\_presets capability has a value of 8.

[1]:	https://snap-one.github.io/docs-driverworks-proxyprotocol-camera-4.0.0-beta/#camera-capabilities-has_dynamic_presets
[2]:	https://snap-one.github.io/docs-driverworks-proxyprotocol-camera-4.0.0-beta/#camera-capabilities-number_presets