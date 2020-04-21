## ComposerPro and Your Driver

The ComposerPro software application will play an important role in your driver development efforts. Before discussing how it can enhance the experience dealers and integrators will have with your driver, it’s important to understand that ComposerPro also:

- Provides the ability for a dealer/integrator to search for and locate your driver.
- Provides the ability for your driver to be added to a Control4 project
- Provides the ability for your driver to be updated when needed.

In developing your driver you’ll want to consider the driver’s Properties. Properties are data values which are used within your driver. They are defined by you in your driver code and exposed in the Composer System Design interface on the Properties tab. It might be helpful to consider the Properties area of ComposerPro as a “Driver Configuration Section”. You control what is displayed in this area of ComposerPro by the code created in the Properties section of your driver’s XML. This provides a way for dealers and integrators to modify how your driver performs within a given Control4 project. For example, our receiver driver may have a way to introduce a delay between commands that are sent to the device. We can create a “Command Delay-Milliseconds” Property which the supports a range of delay from 50 to 2500 milliseconds. The dealer or integrator setting up our driver can set this value to whatever they desire.

ComposerPro also provides a way for you to display supporting Documentation. Quality setup and configuration documentation is an important component of any driver. The Documentation tab in ComposerPro offers you an opportunity to provide consumers of your driver some information that you feel may help them with implementation. The content displayed here is found in the Documentation section of the driver’s XML.

Actions define programming which is specific to initial installation/configuration of your driver. This is typically functionality which is not required for ongoing driver use.

The Action Tab is used to define programming which is specific to initial installation and configuration of the driver. This is typically functionality which is not required for ongoing driver use. For example, consider that our receiver driver happens to use an Action to fire a command. This particular receiver can also be controlled via Serial. In order to turn the receiver “ON” using a serial command, the ENABLE POWER\_ON\_RS232 Action must be fired.

ComposerPro also includes an embedded Lua output window. This is a great way for you or a consumer to test the non XML portion of your driver. You can enter a Lua command and the output of that command is displayed in the Lua Output window. If there are any programming errors, the error messages will also be displayed in this window.

Because the Lua Command window gives you access to an active interpreter running on the Controller, you can modify any of your Lua programming on the fly. You can re-compile functions, correct and improve your programming, and add functionality – all without leaving Composer. This is very useful when prototyping a driver.

ComposerPro also offers the ability to support functionality found in your device that falls outside of the defined Control4 Proxy. Your drive can include unique device commands that can be made available to end users through the Programming UI that is configured within ComposerPro.