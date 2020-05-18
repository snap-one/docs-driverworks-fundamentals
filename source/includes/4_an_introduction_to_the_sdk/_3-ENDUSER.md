## Considering End User Interaction

Navigator is the main vehicle for direct user interaction with the Control4 Operating System. The current iterations of Navigator are:

- Android OS used in Control4 touch screens and in MyHome PC
- iOS Navigator used in Apple Apps
- Android Navigator used in Android Apps\\
- List Navigator used in Control4 Remotes

By selecting Navigator elements, the end user can explicitly control a device (via a DriverWorks driver)  like turning on a light using the light icon from a Touch Panel or changing inputs on a receiver using a Control4 remote. Additionally, implicit control of one or many devices can be affected. For example, a complete Home Theater system can be powered-up, all inputs properly selected and the default volume set, merely by selecting a movie title from a media list. Understanding how Proxy and Protocol drivers participate in these and other interoperability scenarios are key to developing dependable drivers.

Your driver will report back to the Control4 system giving indication of device state (i.e. ON & OFF, current volume, currently select media, etc.) and the of the occurrence of Events (i.e. the station has changed, button has been pressed, etc.). This data may be purely informational or may be used for decision making to initiate of more advanced functionality.

Another means of supporting user interaction and your device is the “Programming UI.” Programming UI refers to the interaction a user has with a device through programmed button presses, events or other means that are configured in the ComposerPro.