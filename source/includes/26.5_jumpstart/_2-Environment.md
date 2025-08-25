
## JumpStartEnvironment.json

The JumpStartEnvironment.json file contains the settings used by JumpStart
to configure the environment for the initial generation of a new
driver. Once configured, the settings in this file will remain consistent for a
given developer’s environment and not need to change with each new
driver generated. If the settings are changed, they will not affect the final driver product, just the processes the developer uses.

A different Json file with the same format can optionally be passed as a
second parameter to JumpStart. If a second Json file is not specified,
JumpStart will attempt to open JumpStartEnvironment.json. If that file
doesn’t exist, then each field will use its default value. For example:


`C:JumpStart.exe DriverFiles/AcmeTV.json AltEnv.json`


The Json elements in the file are:

**General**

The sub-elements in General tell JumpStart where to find information, where to store files, and also include settings for the initial generation of the driver source code.

Possible sub-elements of General are:

_TemplateDir_

This is a string that tells JumpStart where to find the Control 4 template code. JumpStart assumes that drivers will be generated from this code. These files can be located anywhere that can be reached by a valid path string. Copies are  made of the needed template code files and stored in the generated source code directories for the new C4Z driver file. If this element is not included, its default value will be: `“”`.


_WorkDir_

This is a string that specifies the name of the parent directory where JumpStart creates the source code for the new driver. JumpStart creates a directory with the same name as the new driver and then creates driver-specific files in that directory. It then copies the required template directories in as subdirectories. If this value is not included, its default value will be `“”`.


_IncludeLuaCheck_

This is a Boolean string that specifies whether or not JumpStart will generate files to support Luacheck. LuaCheck is a command-line tool for linting and static analysis of Lua Code. It can be found at:

[ https://luarocks.org/modules/mpeterv/luacheck ]()

Some developers have found it useful to use this tool. If this value is “True”,  JumpStart will generate a .luacheckrc file that Luacheck can use to check the source code. It will also generate a validate.bat file that can be used to call Luacheck on all the source files. If this value is not included, its default value will be: `False`. 

For example: 

```json
{
	"General":{
	  "TemplateDir": "..drivers-template-code", 
	  "WorkDir": "NewDrivers",
	  "IncludeLuacheck": "True”,},
	}
  }
```

**Build**

This is an optional entry that can be used to generate files for the developer’s preferred build method. Possible sub-elements of Build are:


_BuildMethod_

This specifies the preferred build program. Supported methods are DriverPackager and CreateC4Z. If this entry is not included, the build method will default to “custom” and no special build files will be generated.

If DriverPackager is specified, a .c4zproj file and a squishy file are generated.

If CreateC4Z is specified, a .c4zmanifest file is generated.


**AdditionalFiles**

This element specifies any additional files that the developer may want to have included in the driver’s source directory. The files are specified by path names that JumpStart can access and are copied into the source directory. Any text strings in the source files of `^@JSPROJ^` will be replaced by the target name of the driver. For example:

```json
"Build": 
{"BuildMethod": "CreateC4Z", "AdditionalFiles": "ExtraBatchFiles\dev.bat", "ExtraBatchFiles\rel.bat", "ExtraBatchFiles\rel.sh"},
```






