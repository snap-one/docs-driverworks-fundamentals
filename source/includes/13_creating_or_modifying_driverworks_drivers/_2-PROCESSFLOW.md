## Development Process Flow

### Obtain the device and control modules (if needed).
If you are creating a driver for a device, you need to have access to the device itself and to the accessory modules (if required) that enable it to communicate via serial or TCP/IP with a control system. 

### Obtain the control protocol documentation.
The control protocol documentation for the device will contain all of the information needed to implement your DriverWorks driver. This should include control protocol, communication protocol. 

### Physically connect the device to your Control4 system.
Determine how the device will communicate with the Control4 system. With DriverWorks, you can use either serial or TCP/IP communication protocols. Make the physical connections between your device and the system.

### Select the appropriate proxy type for your custom driver.
The proxy selection determines the interface, which will be automatically generated for the user of the system in controlling this device.

### Decide whether to use a template .c4z or generate a .c4z using the Driver Wizard.
While you can hand code the entire .c4z file with the appropriate information, you’ll find it easier to start with a pre-formed .c4i. The advantage of using a template .c4z is it is already set up for DriverWorks and allows you to add your custom programming and modifications.  However, particularly with more complex AV devices containing multiple inputs / outputs, you may find it easier to generate a .c4z file using the Control4 Driver Wizard and then modify that .c4i file for use with DriverWorks. If an existing driver is available, you can modify that driver to meet your requirements. This assumes a driver already exists for the device.

### Select an editor.
The DriverWorks creation process requires that you use an editor. You can use a text editor, and/or an XML editor, and (if desired) an IDE such as DriverEditor for creating your DriverWorks driver.  The advantage to a text editor is that it allows for the greatest flexibility in text entry. However, you have to be careful to ensure that the XML structure of the .c4z file is correct.The reason to consider an XML editor – at least for checking your .c4z file – is that it will help you identify if you have XML construct errors. The .c4z file needs to be proper XML for it to work within the Control4 system. Depending on the complexity of your Lua programming, you might also find it helpful to utilize an IDE which supports Lua.  This will allow you to validate your programming for errors prior to inclusion in the driver. Alternatively, you can also use the Lua interpreter built into Composer for that purpose.

### Create/edit your driver.
Obviously, this is the heart of the process and may require a considerable number of iterations to get correct. Make backups of your driver along the way so that if you need to recover to a known good point, you can easily do so. You will find it beneficial to start simple, get the basic driver working, and then add functionality.  We’ll cover this procedure in detail in a later section.

### Add your driver to the Control4 project.
 In order to test your driver you will add it to the Control4 system.

### Update the driver loaded within your project.
If you are just making changes to the Lua programming, you can use the Composer Lua window to continually update and refine the functionality of your driver without reloading.  However, changes to other areas of the .c4i file (such as Commands, Properties, Events, Capabilities, etc) will require that the .c4i be reloaded in the project in order to have access to the changes you are making. If you don’t want to redo your programming and bindings, the easiest way to reload your driver is to load a new instance of that driver into your project and then restart Director.  Even if you have just been updating the Lua code, you need to reload the final .c4i file when you are done so that when Director restarts you will have the latest iteration.

### Share your driver.
If you want to share your driver with someone else, simply e-mail (or otherwise transfer) the .c4z file to them.  As long as they are running a 1.6 or later system, all they have to do is use Composer to add the custom driver to their system.

