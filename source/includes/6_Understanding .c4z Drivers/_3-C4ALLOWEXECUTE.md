## C4:AllowExecute API

The C4:AllowExecute API is a function which dictates whether or not the lua window in ComposerPro can be used to view debugging content or exercising functions within a driver. 

C4:AllowExecute defaults to false which locks the driver’s Lua execute and output window.  If a driver is encrypted, C4:AllowExecute defaults to locked (False)

However, if this API is set to true the lua command window is unlocked and supports execution of data and results being displayed in the lua output window.

Entering the following in the driver’s Lua execute window will lock the lua window: `C4:AllowExecute(false)`.

A more preferable approach to using the API is to create an Action that can change the command dynamically.

The C4:AllowExecute API is evoked in OnDriverInit or OnDriverEarlyInit.
