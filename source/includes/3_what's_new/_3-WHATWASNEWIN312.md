## What was New in 3.1.2

### A Note to DriverEditor Users

Beginning with OS 3, Director no longer accepts connections on its unsecure port. Previously, DriverEditor 3.0.1 used that port. Control4 has not yet updated DriverEditor to use Director’s secure port for the debug connection. In the interim, the Lua Input window within Composer Pro is available to make code changes and test driver functionality. This method is useful in making point changes to the running code on the controller and also includes pasting and executing complete functions or even your entire driver, while you are developing. 

An important caveat regarding this debug approach must be considered: The driver must be re-built the and updated with code changes before finishing the Lua debug window development. Any code changes made in the runtime environement  will only last for the duration of the uptime of Director. As soon as Director is restarted, the driver will be reloaded from the copy on disk and changes will be lost.