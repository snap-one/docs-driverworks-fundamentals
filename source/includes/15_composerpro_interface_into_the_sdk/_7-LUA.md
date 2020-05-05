## LUA

All properties can have initial/default values and may be changed by the installer or configured as read-only

The Lua tab contains two windows.  The Lua Command window where Lua commands can be entered and executed.  The results of those commands will be displayed in the Lua Output window.
In the figure below, very simple lines of Lua programming have been entered. The first line is the familiar “Hello World” programming used to introduce all programming languages. The second line combines a text string with a string from the property page (Your Name).

![]()

When this programming is executed, the output of both commands is presented in the Lua Output window.  If there are any programming errors, the error messages will also be displayed in that window.
Because the Lua Command window gives you access to an active interpreter running on the Controller, you can modify any of your Lua programming on the fly. You can re-define functions, correct and improve your programming, and add functionality – all without leaving Composer. 
You can navigate through the tabs: Properties, Documentation, Actions etc. without losing information displayed in the Lua Output window.
However,  it is important to understand that the data displayed in the Lua Output window is not saved. **If you navigate away from the device without saving any needed information, you will lose your Lua output.**

