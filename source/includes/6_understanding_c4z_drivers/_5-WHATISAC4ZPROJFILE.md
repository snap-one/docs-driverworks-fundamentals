## What is a .c4zproj File

As described in the Understanding .c4z Files, a .c4z file is a zipped filed consisting of several directories and numerous files that (when zipped together) represent a device driver. DriverEditor views these assembled files as a "Project." When a .c4z file is imported or opened through the DriverEditor environment, a corresponding .c4zproj file is created. For example, let's assume the driver you wish to import into DriverEditor resides at the following location:

`C:\Users\username.CONTROL4\Documents\Control4\Drivers`

This the Control4 system default location for DriverWorks drivers. The example directory looks like this:

todo img 7

If we import the HelloWorld.c4z file into the Driver Editor environment, we can go back and see that a new directory has been created:

todo img 8

The HelloWorld File Folder was created by DriverEditor. All of the changes made to the .c4z file through DriverEditor will be made to this directory. If we open the new Hello World folder we'll find this:

todo img 9


Note the addition of the HelloWorld.c4zproj file. This file allows DriverEditor to assemble, encrypt, name the .c4z file along with defining several other file level functions. If we open the .c4zproj file we'd find the example to the right:

```xml
<Driver type="c4z" name="sample" squishLua="true" Encryption="True/False">
<Items>
 <Item type="dir" name="www" recurse="true"/>
 <Item type="dir" name="common" c4zDir="Common"/>
 <Item type="dir" name="tests" exclude="true"/>
 <Item type="file" name="driver.xml"/>
 <Item type="file" name="squish.lua"/>
 <Item type="file" name="foo1.lua"/>
 <Item type="file" name="foo2.lua"/>
 <Item type="file" name="library.lua" c4zDir="Common"/>
 <Item type="file" name="readme.txt" exclude="true"/>
 </Items
</Driver>
```

The first line of the proj file contains the following:
**Driver type** – This must be “c4z” for the manifest to be valid.

**name** – This is the name of the driver in quotes  without its extension.

**squishLua** – Must be set to “true” or “false”. It defaults to a value of "false." There are two options when encrypting a driver: encrypt a single Lua file or Squish all Lua files into one file and encrypt it. Squish is a tool that packs many individual Lua scripts and their respective modules into a single Lua script. A file called “squishy” must be created for the squish tool. This file contains all of the Lua files to be included in the squished file. Here is an example of a basic squishy file:


`Main "driver.lua"
`
`Module "module1"
``Module "module2"
``Module "common.command" "common/command.lua"
``Module "common.common" "common/common.lua"
``Module "common.diagnostics" "common/diagnostics.lua"
`
`Output "squished.lua"
`
**Encryption** – Designates whether or not the driver will be encrypted or not.

Next you'll notice the \<Items\>\</Items\> section:
**Item type** - Must be “dir” or “file”. This specifies if the item is a file or a directory. “dir” creates a folder 'name' and adds all immediate files beneath 'name' to the c4z. “file” adds file 'name' to the c4z.

**Item name** - Name of folder or file to be added to c4z.

**recurse** - Optional. Only applicable to type 'dir' items. Must be "true" or "false", default is "false" if not specified. If "true", recursively adds all files beneath 'name' to c4z. 

**c4zDir** - Optional. The name of the c4z folder where the 'dir' or 'file' item is added. 

**exclude** - Optional. Must be "true" of "false", default is "false" if not specified. This specifies if an item is excluded from the c4z. 

