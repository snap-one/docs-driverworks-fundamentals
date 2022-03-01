## SquishLua and Driver Encryption

All .lua files included in a .c4z file must be squished prior to encryption. This is accomplished using Squish Lua. Squish is a tool that can pack many individual Lua scripts and their modules into a single Lua script. Whether or not Squish is run on the .lua files inside of a .c4z file is defined in the .c4z project file. in the example project file below you can see that the squishLua parameter has a value of "true." With this setting, Driver Packager will squish all of the .lua files before encryption. This is a requirement for successful driver encryption. See the example to the right.

```xml
<Driver type="c4z" name="sample" squishLua="true" Encryption="True" >
<Items>
 <Item type="dir" name="www" recurse="true" />
 <Item type="dir" name="common" c4zDir="Common" />
 <Item type="dir" name="tests" exclude="true" />
 <Item type="file" name="driver.xml"/>
 <Item type="file" name="squish.lua" />
 <Item type="file" name="foo1.lua" />
 <Item type="file" name="foo2.lua" />
 <Item type="file" name="library.lua" c4zDir="Common" />
 <Item type="file" name="readme.txt" exclude="true" />
</Items>
```


The first line of the .proj file contains the following:

**Driver type** – This must be “c4z” for the manifest to be valid.

**name** – This is the name of the driver in quotes  without its extension.

**squishLua** – Must be set to “true” or “false”. It defaults to a value of "false." There are two options when encrypting a driver: encrypt a single Lua file or Squish all Lua files into one file and encrypt it. Squish is a tool that packs many individual Lua scripts and their modules into a single Lua script. A file called “squishy” must be created for the squish tool. This file contains all of the Lua files to be included in the squished file. Here is an example of a basic squishy file:

Main "driver.lua"

- Module "module1"
- Module "module2"
- Module "common.command" "common/command.lua"
- Module "common.common"  "common/common.lua"
- Module "common.diagnostics" "common/diagnostics.lua"

Output "squished.lua"


**Encryption** – Designates whether or not the driver will be encrypted or not.

Next you’ll notice the `<Items></Items>` section:
**Item type** - Must be “dir” or “file”. This specifies if the item is a file or a directory. “dir” creates a folder 'name' and adds all immediate files beneath 'name' to the c4z. “file” adds file 'name' to the c4z.

**Item name** - Name of folder or file to be added to c4z.

**recurse **- Optional. Only applicable to type 'dir' items. Must be "true" or "false", default is "false" if not specified. If "true", recursively adds all files beneath 'name' to c4z. 

**c4zDir** - Optional. The name of the c4z folder where the 'dir' or 'file' item is added. 

**exclude** - Optional.  Must be "true" of "false", default is "false" if not specified. This specifies if an item is excluded from the c4z. 


### Usage Note

Driver Packager is a Python utility used to create individual .c4z files from source code. When Driver Packager assembles a .c4z file and the Encryption parameter in the .c4z project file is set to true (Encryption="True"), the file is encrypted using a new and improved encryption protocol based on an asymmetrical public key infrastructure. This level of encryption is applicable to .c4z files only.

For more information please see: [https://github.com/control4/drivers-driverpackager][1]

[1]:	https://github.com/control4/drivers-driverpackager