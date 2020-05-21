## Using the COLOR SELECTOR Property

The ability to assign a color to interface elements through a device driver is possible with the use of a DriverWorks Property Type called `COLOR_SELECTOR`. This property has most often been used for backlight purposes on thermostats or LED lights on Thermostat drivers but it’s applicable to any driver supporting color based interfaces. A prototype of the property looks like this:

```xml
<property>
  <name>Test Color Selector</name>
  <type>COLOR_SELECTOR</type>
  <default>255,0,0</default>
</property>
```

The property contains a comma delimited RGB value which the driver receives through the OnPropertyChanged event. OnPropertyChanged is invoked when the user defines a color within the Color Selector and hits the Set button from within Composer Pro:

todo img 1

Note that the property defaults to 255, 0 ,0 which is red. The `COLOR_SELECTOR` property will accept any comma delimited RGB value as an entry. To change the color used by the property, click on the Test Color Selector block which displays the currently assigned color:

todo img 2

Selecting the box will display the color selection palette:

todo img 3

The palette provides the ability to select a basic color, assign a color from the spectrum field and save those colors as “Custom Colors” .

After a color has been selected, clicking OK will close the color selection palette screen and populate the` COLOR_SELECTOR` property in ComposerPro with a Set or Cancel button.

Selecting the Set button will assign the color to the property. A color can also be assigned to the `COLOR_SELECTOR `property under the Programming/Actions area of ComposerPro. Clicking on Programming will display the following Actions screen.

todo img 4

The Test `COLOR_SELECTOR` under Actions works in a similar manner as it does under properties. To assign a color, click on the Test Color Selector block which displays the currently assigned color. Selecting the box will display the color selection palette and a color can be selected as in the previous example. 
This creates an Action with a color assignment that can be used in system programming. For example: “When this button is pressed set led color to green.”

