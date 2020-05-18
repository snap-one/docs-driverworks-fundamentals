## Overview

Beginning with operating system 2.6.0, Control4 has changed the file structure previously used to support device drivers. Up until 2.6.0, device driver were contained within a single-level file using the extension of .c4i or: drivername.c4i

The release of 2.6.0 marked a significant change to this model with the introduction of the .c4z file structure or: drivername.c4z. A .c4z file provides all of the content that the .c4i file previously did. However, it offers some significant features and advantages that its predecessor did not.

At its most basic level, a .c4z file is a zip file. It contains numerous folders and files which, when encapsulated in a .c4z file, represent a device driver. The modularity within the .c4z file makes for a far more organized and structured approach to device driver architecture. It also provides the ability to include many related objects within the confines of the driver in an organized manner.

These objects include items such as graphics and icon directories to support custom user interfaces which can run on Navigator. A .c4z file also contains a .lua file which contains all of the driver’s lua code. It has its own .xml directory as well to separate the XML portion of a driver from the .lua code. Also, rich text documentation can be included in the .c4z to support a far superior user assistance model.

The image below is a look into a .c4z file at its root level:


todo img 1


You’ll notice several files at the root level of the .c4z which were described in the section above. The www folder must contain the documentation file to support the driver. 

todo img 2

This is an architectural change implemented with the release of OS 2.8.1. 

This is the driver’s overview and configuration documentation that integrators will see displayed in ComposerPro and ComposerExpress. Note that the inclusion of the .RTF file in the .c4z  ensures that the information is displayed in ComposerPro only. In order for your documentation to be displayed in ComposerExpress, you will need to also enclose the text within the  \<documentation\> XML tags in the .c4z file. 

For example:
\<documentation file="doc.rtf"\> This is text that will appear in Composer Express. \</documentation\> 

Using this structure, ComposerPro will load the RTF documentation file and ignore the text between the XML tags. Composer Express, which can't display RTF documentation, will display the text documentation within the XML tags.

Below that is the .lua file that contains all of the .lua code for the driver. It is possible to have multiple .lua files included in the .c4z file. For example, here is an opened .c4z file for a pool controller: 

todo img 3

As you can see, this .c4z contains numerous .lua files. In order for all of the .lua files to be recognized not just by DriverEditor but Director as well - the use of the Lua require function is needed. The required Function loads and runs libraries. All of the .lua files that need to be included in .c4z file are identified in the .c4z file's driver. lua file. If we open the pool controller's driver.lua file we see this: 

todo img 4

In the example above, the file names are surrounded by quotation marks. The require Function uses the global variable LUA\_PATH to find the file. This variable is defined as LUA\_PATH = C4Sytem; Driver. Based on this, Director will look for the .lua file on the controller first, The LUA\_PATH environment on the controller is:
/control4/drivers/lua/?.lua; 
As each driver contains its own environment the LUA\_PATH for each driver would be: 

LUA\_PATH=/control4/drivers/lua/?.lua;/etc/c4i/test\_driver\_name/?.lua;

The require Function can also navigate a path to the .lua file if it is defined. For example:
require "/usr/local/lua/pool\_init.lua"
When Director loads this .c4z file it will load all of the required .lua files into memory as one large .lua file and execute based on the contents of the assembled file. 
lua files can be included in a .c4z, defined using the require Function and still be excluded when DriverPackager assembles the final .c4z. This is useful in the event that a .lua file was included for testing purposes but is not needed when the final .c4z is delivered. This is handled in the .c4zproj file. Specifically, with the exclude parameter. For example, say a test.lua file was included in our .c4z, but is not needed when the driver is delivered. The .c4zproj file would look like this:

`<Items>`
`<Item type="dir" name="www" recurse="true"/>`
`<Item type="dir" name="common" c4zDir="Common"/>`
`<Item type="dir" name="tests" exclude="true"/>`
`<Item type="file" name="driver.xml"/>`
`<Item type="file" name="squish.lua" />`
`<Item type="file" name="foo1.lua" />`
`<Item type="file" name="foo2.lua" />`
`<Item type="file" name="library.lua" c4zDir="Common"/>`
`<Item type="file" name="test.lua" exclude="true"/>`
`</Items>`

Note the test.lua line with exclude set to True. This is will prevent this file from being packaged by DriverPackager.

Next we can see another driver file. This contains all of the XML that was previously found between the \<devicedata\> tags of a .c4i file. These are elements such as \<creator\>, \<name\>, \<model\>, \<manufacturer\>, \<identify\_image\>, \<identify\_text\> and so on.
If we open the www directory we see the following:

todo img 5

As mentioned above, a c4z file can contain graphical elements to support the driver’s use in Navigator. When opened, the icons folder for this driver looks like this:

todo img 6

The icons directory contains all of the images, organized by their resolutions, which are displayed during the use of the driver. The images in this particular driver are found under the root level of “www” placing them within the .c4z in this manner makes them accessible from via the controller’s webserver. For example, accessing an image can be accomplished by appending the .c4z icon path to a URL such as: 

`http://urlstring/driver/drivername/icons/20x20/driverimage.png`
or
`http://127.0.0.1/driver/myc4zdriver/icons/20X20/driverimage.png`

The other directory file found under this driver’s www directory is the languages folder. This folder contains all of the .po files used for localizing this driver.

Going forward, any .lua-based driver will be expected to be delivered in the .c4z format. To facilitate the conversion of previously built .c4i files to the new .c4z format, control4 has delivered a utility called DriverPackager. DriverPackager accomplishes two significant tasks for the developer. These include:

- XML Validation
- Assembles the .c4z

