
## Light Proxy Driver Development Best Practices

The purpose of this section is to provide best practice advice for driver developers developing lighting drivers using the Light V2 Proxy. By following these recommendations, developers will ensure consistent behavior from their driver and other lighting drivers. Additionally, implementing these guidelines will greatly improve driver interaction with other components in the Control4 OS such as Agents, Customer Interfaces, Programming, etc.

For documentation that details the functions that make up the Light V2 Proxy, please see the [Light Proxy and Protocol Guide][1].

#### Brightness Target API
A Brightness Target API has been released in conjunction with O.S. 3.3.0. The API is disabled by default for non-color capable drivers and can be enabled with the [supports\_target][2] capability. The API includes the [SET\_BRIGHTNESS\_TARGET][3] proxy command and [LIGHT\_BRIGHTNESS\_CHANGED][4] and [LIGHT\_BRIGHTNESS\_CHANGING][5] proxy notifies.

Brightness target API should be used by all new driver written against the Light V2 Proxy and existing drivers should be updated if possible. The API is required for driver certification in 3.3.0.  Dimmer drivers should now only use LIGHT\_BRIGHTNESS\_CHANGING and LIGHT\_BRIGHTNESS\_CHANGED notifies. Switch drivers only need to use the BRIGHTNESS\_CHANGED notify. Note that the LEVEL\_CHANGED notify should not be used.

Using the LIGHT\_BRIGHTNESS\_CHANGING notify command allows drivers to inform the proxy of brightness transitions. Customer Interfaces monitor LIGHT\_BRIGHTNESS\_CHANGING events and will display a brightness changing animation depending on the provided current brightness, target brightness and rate. This can be useful for devices that do not send intermediate status notifications when transitioning between brightness levels. It is also recommended to send brightness changing notify command in cases where the final level is unknown, such as holding a button to ramp a light, scene or group up or down. For example, if a customer holds a button to slow-ramp a light on, the driver should notify the proxy that the brightness is changing to 100%. This will in turn show a brightness changing animation on Customer Interfaces. Once the customer releases the button (e.g. at 30%), the driver should send a LIGHT\_BRIGHTNESS\_CHANGED notify to indicate where the light ramping stopped.

Previously, drivers had to 'emulate' slider movements on Customer Interfaces by sending periodic, incremental light level notifies during brightness transitions (e.g. every 20ms). These updates cause performance issues including:

- Slow performance in proxies, director, customer interfaces and Advanced Lighting Scene agent.
- Potentially inaccurate states for Advanced Lighting Scene tracking.
- Excessive zigbee traffic and often incorrect keypad button LED status.
- Inconsistent slider movement in Customer Interfaces.
- Excessive triggering and potentially incorrect Composer Programming being fired.

#### Brightness and Color Changing API
OS 3.3.0 also introduced a new Light V2 Proxy API that allows protocol drivers to notify the proxy that a light's color ([LIGHT\_COLOR\_CHANGING][6]) or brightness ([LIGHT\_BRIGHTNESS\_CHANGING][7]) is changing. The use of these notify commands are optional. However, it is recommended to notify the proxy of color or brightness transitions whenever possible. Using these notify commands allows other drivers or agents in the system to monitor the state of the light.

Using these notifies also allows additional Programming:

- Events: Brightness Begins to ramp and Color begins to ramp.
- Conditionals and Loops: Is Brightness Target and Is Color Target.

#### Enforcing min/max Capabilities
Beginning with OS 3.3.0, Advanced Lighting Scenes Agent, Customer Interfaces, Programming and other system components strictly enforce min/max and other capability settings in LightV2 Proxy drivers. It is recommended to verify if these capabilities are correct in your drivers.  Existing out-of-range settings (e.g. in [Advanced Lighting Scenes agent][8], Programming, etc.) will not be forcibly changed on upgrades but any new settings will enforce min/max capabilities.

For example, in pre-OS 3.3.0, Composer and the proxy only enforced [click\_rate\_min/max capabilities][9] in a driver's Properties. In the Advanced Lighting Scene Agent and Programming, it was possible to set a rate that was out of the click\_rate\_min/max range. In OS 3.3.0 and later, it will not be possible to set a rate out of this range. If these capabilities are not correct, partners and customers will not be able to use correct range when specifying ramp rates.

#### Handling Consecutive Brightness and Color commands
The Light V2 Proxy has independent commands to set color (chromaticity) ([SET\_COLOR\_TARGET][10]) and brightness ([SET\_BRIGHTNESS\_TARGET][11]). It is possible to send these commands to the protocol driver one immediately after the other (e.g, Programming, Customer Interfaces, Agents or other drivers). When these commands come in succession, a desired outcome is that the light transitions to the specified color and brightness, i.e., consecutive commands should not interfere or override each other.

Driver developers must make sure that consecutive color and brightness commands are handled correctly. If the device does not support consecutive commands, a best practice is to have a debouncing (and/or queuing) mechanism in the driver so that commands can be combined.


#### Default On and Dim Colors
All colored lights have a mandatory Default On Color and Default Dim Color presets. These provide a consistent behavior when turning the light on or off. Furthermore, these presets can also be used as 'hooks' for other agents/drivers in the system that might want to change color that the light should turn on to or transition to while turning off.

Default On Color should be set whenever the devices turns on through [SET\_BRIGHTNESS\_TARGET][12] command, Button Click and Hold actions, etc.

Default Dim color should be used while the light is turning off, except in cases when the final level is unknown. For example, color should not be changed while dimming down using button hold since it is not known when the dimming will be stopped. For example, it is not known if the customer is dimming to turn off the light or to only lower the brightness.

#### Setting Color while the Light is Off
The Light V2 proxy allows sending [SET\_COLOR\_TARGET][13] commands to the protocol driver while the light is off. In Control4 OS. color chromaticity and brightness are independent. Because of this, the protocol driver should not turn on the light when a command to set the color is received while the light is off. Instead, drivers should save the last color that was sent while the light is off and set that color when the light subsequently turns on from the Control4 system.

#### Warm Dim
Warm Dim technology allows LED lights to dim down to a warmer color temperature. This replicates the effect of incandescent and halogen light bulbs. With color and color temperature support in OS 3.3.0, driver developers can 'emulate' Warm Dim behavior in their drivers. The following section explains how Warm Dimming is implemented for a Philips Hue driver. However, this approach is recommended for other drivers that support this behavior.

**Principle of Operation for Philips Hue**
This feature is only available for lights that support color correlated temperature (CCT) control. Warm Dimming can be enabled or disabled on the Lighting Extras view on the Customer Interfaces or on the driver's Advanced Properties page in Composer. When enabled, a change to the light's brightness level will automatically change the color temperature of the light so that lower brightness results in warmer colors and vise versa.

When Warm Dimming is active, color correlated temperature of the light will follow a linear function of brightness, between the 'Warm Dimming CCT At Min Brightness' and 'Warm Dimming CCT At Max Brightness' Advanced Properties. 'Warm Dimming CCT At Min Brightness' and 'Warm Dimming CCT At Max Brightness' are color correlated temperatures of the light at 1% and 100% brightness, respectively:

`cct = WarmDimmingCCTatMinBrightness + (targetBrightness - 1) * (WarmDimmingCCTatMaxBrightness - WarmDimmingCCTatMinBrightness) / 99`

Enabling Warm Dimming does not change Default On Color and Default Dim Color settings. The light will turn on at the Default On Brightness and CCT will be the value for Default On Brightness on the warm dimming function.

The Following graph displays a Warm Dimming CCT change as function of brightness for the following settings:

- Warm Dimming CCT At Min Brightness = 2000K
- Warm Dimming CCT At Max Brightness = 6000K
- Default On Brightness = 50%
- With brightness change to 1%, CCT will also be set by the driver to 2000K. At full brightness (100%), CCT will be 6000K. When the light is turned on, CCT will be 3980K (at Default On Brightness of 50%).

Warm Dimming will be active only while changing brightness of the light. Changing the CCT does not affect brightness. Furthermore, if any color is selected (including CCT,  e.g. via Customer Interfaces, Advanced Lighting Scenes, Programming) the light will transition to the specified color and will exit the 'emulated warm dimming' mode. To return a light to the emulated warm dimming mode, it must be turned off and then turned back on.

For additional Information, please see the [DriverWorks Proxy and Protocol Guide][14] regarding all of the functions supported by the Light Proxy.

[1]:	https://expert-adventure-1w2nllv.pages.github.io/#introduction
[2]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#light-v2-capabilities-supports_target
[3]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-commands-set_brightness_target
[4]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-protocol-notifications-light_brightness_changed
[5]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-protocol-notifications-light_brightness_changing
[6]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-protocol-notifications-light_color_changing
[7]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-protocol-notifications-light_brightness_changing
[8]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#advanced-lighting-scene-agent
[9]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-capabilities-click_rate_min
[10]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-commands-set_color_target
[11]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-commands-set_brightness_target
[12]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-commands-set_brightness_target
[13]:	https://expert-adventure-1w2nllv.pages.github.io/#light-v2-commands-set_color_target
[14]:	https://expert-adventure-1w2nllv.pages.github.io/#introduction