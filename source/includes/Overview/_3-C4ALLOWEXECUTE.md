## C4:AllowExecute API

The C4:AllowExecute API is a function which dictates whether or not the lua window in ComposerPro can be used to view debugging content or exercising functions within a driver. C4:AllowExecute defaults to false which allows for the normal operation of the lua window. However, if this API is set to true, the lua command window will not support entry of any data and the lua output window cannot be used as a display. If a driver is encrypted C4: AllowExecute defaults to locked (True)
Setting the function to true has numerous implications. In its simplest use case, entering the following in the driver’s Lua execute window will lock the lua window: C4: AllowExecute (true).

A more preferable approach to using the API is to create an Action that can change the command dynamically.

The C4:AllowExecute API is evoked in OnDriverInit or OnDriverEarlyInit.
