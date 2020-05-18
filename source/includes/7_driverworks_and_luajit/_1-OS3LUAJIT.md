## Control4 OS 3 and LuaJIT

Prior to OS 3, Control4 loaded drivers into the controller environment using the Lua run time compiler known as "standard Lua" or "PUC Lua". With the release of OS 3, support for Lua's Just In Time compiler (LuaJIT) has been added. Note that LuaJIT compatibility is an addition and not a replacement for PUC Lua. This means that driver modification is not a requirement for OS 3 compatibility. PUC Lua will continue to load previously released drivers as it has in the past.

Modifying a driver to use the LuaJIT compiler is worth consideration. LuaJIT is considerably faster that PUC Lua. The speed enhancement it provides offers a direct benefit to end users through increased responsiveness and scalability. This is particularly beneficial when considering your drivers deployment into larger systems. 

A considerable amount of information is available regarding LuaJIT on the Lua homepage and the LuaJIT wiki:

https://www.lua.org/

http://wiki.luajit.org/Home


### Modifying your Driver to use LuaJIT

Beginning with OS 3, Director looks for a LuaJIT XML attribute within the driver. If Director does not detect the presence of the attribute it loads it using the PUC Lua compiler. If a LuaJIT XML attribute is specified in driver.xml, it loads the driver using the LuaJIT compiler.

In order for your driver to be loaded using the LuaJIT compiler it will be necessary to modify your driver.xml to include the LuaJIT XML attribute in the driver's XML code. This attribute is added to the driver's script XML element. A value of "1" specifies that the driver should be loaded with LuaJIT. A value of "0" (or lack of the attribute altogether) will result in PUC Lua loading the driver. For example:

Driver loads using LuaJIT: `<script encryption="2" file="driver.lua" jit="1"/>`
Driver loads using PUC Lua: `<script encryption="2" file="driver.lua" jit="0"/>`

After your driver has been modified to include the LuaJIT XML attribute and it contains a value of "1" a restart of Director is required. Director will then attempt to load the driver using LuaJIT. If LuaJIT indicates that syntax errors occurred during the load process Director will destroy the driver instance and then reload the driver using the PUC Lua compiler. 

Note: There is no fall back to using the PUC Lua compiler for drivers that were initially loaded using LuaJIT. This means that if a driver has errors it will fail to load - even if it may load successfully using PUC Lua.


### Driver Debug Messaging Considerations and LuaJIT

Beginning with OS 3, the ability to send debug Lua messages upon a driver failing to update has changed. Sending Lua debug messages are determined on how the driver's properties are set. When a driver is updated from using the previous PUC Lua compiler to LuaJIT, the driver's state and properties are temporarily lost. This is also true if a driver is updated involves going from LuaJIT to PUC Lua. The state and properties are not restored until the driver has loaded successfully. During this time frame, driver debug messages are not sent. The following are use cases to consider regarding this:


### Driver updating from PUC to PUC.

This is a driver that was initially loaded with PUC and the updated driver likewise loads with PUC. There is no "jit" attribute applied to the driver script. In this case, the same driver instance is reused and the driver will function as it did prior to 3.x. 


### Driver updating from PUC to LuaJIT:

This is a driver that was initially loaded with PUC and the updated driver expects to load with LuaJIT. The specified "jit" attribute has been applied to the driver script. In this case, the old driver instance is destroyed and then re-loaded with LuaJIT. In doing so, all of the driver properties that set for this driver are temporarily lost. This results in the following two scenarios:
If the driver loads successfully all of the properties & state data is restored.
If the driver fails to load successfully, the LuaJIT instance is abandoned and then reloaded with PUC Lua. The properties & state data is restored after the driver loads. Debug Lua messaging is  impacted.


### Driver updating from LuaJIT to LuaJIT:

This is a driver that was initially loaded with LuaJIT and the updated driver likewise loads with LuaJIT. The specified "jit" attribute has been applied to the driver script. In this case,an attempt is made to re-use the same driver instance. This results in the following two scenarios:
If the driver loads successfully, then this will function as it did prior to 3.x.
If the driver fails to load, the LuaJIT instance is abandoned and reloaded with PUC. When this happens, all of the properties that are set for the driver are temporarily lost. The properties & state data is restored after the driver loads. Debug Lua messaging is impacted.

### Areas Where your Driver May Fail

As mentioned above, Director will abandon attempts to load a driver with LuaJit and fallback to PUC Lua when syntax errors occur during the loading of the driver. However, the presence of other run-time errors will prevent a driver from working properly within LuaJIT. If this occurs, the driver’s Lua code will need to be updated to resolve the Lua syntax errors or the driver.xml should not specify that the driver is supported in LuaJIT. 

Many of these errors result from LuaJit's compatibility requirement with the Lua 5.1 language standard. PUC Lua is compatible with version 5.0. Two common errors that are seen in Control4 device drivers include the use of invalid arguments and invalid escape strings:

Invalid Arguments:
In the Lua 5.1 language standard the pseudo-argument arg is no longer supported. Drivers loaded using LuaJit that contain the argument will fail with a run-time error and not be loaded. The following list shows some examples of 5.0 `arg` use and how they can be modified to be 5.1 compatible:

| 5.0 Syntax | 5.1 Syntax |
| --- | --- |
|` #arg` |  `select("#", ...)`|
| `arg.n` | `select("#", …)`|
| `unpack.(arg)` | `unpack({...})`|

Replacements for individual arg syntax lines will resolve this issue. However, it may be more effective to convert the vararg ... into a local variable named 'arg' at the beginning of the function where the arg parameter is used:

`function test(...)`
 ` local arg = {...}`

 ` -- Now all the arg-based code will work properly again...`
 ` print(#arg)`
 ` for i = 1, #arg do`
    `print(arg[i])`
 ` end`
    `print(unpack(arg))`
`end`


### Invalid Escape Sequences
Also due to the Lua 5.1 language standard requirement, invalid escape sequences in Lua 5.0 are not supported by LuaJIT. This means that invalid escape sequences which were previously handled by PUC Lua are not handled by LuaJIT. The resulting error will prevent a driver from successfully loading or running. 

The following table shows two examples of 5.0 escape sequences and how to fix them to meet the 5.1 language standard:

| 5.0 Syntax | 5.1 Syntax |
| --- | --- |
| `string.gsub(s, '\&quot\;', '"')` | `string.gsub (s, '%&quot%;' , '"') string.gsub (s, '&quot;' , '"')` |
| `string.match(C4:GetVersionInfo().version, '(%d+)\.(%d+)\.(%d+)\.(%d+)')` | `string.match(C4:GetVersionInfo().version, '(%d+)%.(%d+)%.(%d+)%.(%d+)')` |

_The Lua 5.1 Language Manual details its incompatibilities with previous versions. For more information see Section #7 of the manual here: http://www.lua.org/manual/5.1/manual.html_


### Order Dependency in Lua Tables

As best practice, Control4 recommends against the use of Lua Tables that depend on consistency with regards to the ordering of pairs. It is important to understand that LuaJIT does not iterate through a table of functions in the same way that PUC Lua does. For example, consider a table of functions that is iterated through using OnDriverInit() with the following code:

`for k,v in pairs(PROTOCOL_DECLARATIONS) do`
`if (PROTOCOL_DECLARATIONS[k] ~= nil and type(PROTOCOL_DECLARATIONS[k]) == "function") then`
`print(k,v)`             
`PROTOCOL_DECLARATIONS[k]()`
`end`
`end`

There is no guarantee that that order of iteration through this table will be the same in LuaPUC and LuaJIT. A dependency on order in the table will likely result in a run-time Lua error. 


### Identifying Related Entries in the Driver Debug logs
When Director attempts to load a driver it reports various errors and conditions to both the director.log and the `driver_debug.log` or: 

`/var/log/debug/director.log`
`/mtn/internal/log/driver_debug.log`

Note that the director.log rotates out its content frequently so driver errors are quickly lost. However, `driver_debug.log` should have very little content and will likely be easier to inspect.   `driver_debug.log` resides in a different directory as it is not cleared on controller reboots. By default, `driver_debug_nl` is set at Error level logging and won’t capture debug log statements. This can be changed to capture debug level for development testing with the following command: sysman log `driver_debug_nl` debug

_For further information regarding system logging, please see the System Manager Logging information._

For reference, a list of sample load conditions and their respective log entries follows:


Condition : A Lua driver loaded successfully with LuaJIT	

Sample Entry: `2018-10-10 12:34:56.789 -0600 ea5-000DEADBEEF [1234] DEBUG: Lua driver loaded successful with LuaJIT [id: 42][name: HeloWorld Driver][file: HeloWorld.c4z]`


Condition : A Lua driver loaded successful with LuaJIT, but there are runtime errors: 

Sample Entry : `2018-10-10 12:34:56.789 -0600 ea5-000DEADBEEF [1234] ERROR: Lua driver loaded with LuaJIT but there were one, or more, runtime errors. This driver may not function correctly [id: 42][name: HeloWorld Driver][file: HeloWorld.c4z]`


Condition : A Lua driver failed to load with LuaJIT due to syntax errors. Director will proceed to reload the driver with PUC Lua:

Sample Entry : `2018-10-10 12:34:56.789 -0600 ea5-000DEADBEEF [1234] ERROR: Lua driver failed to load with LuaJIT; retrying with PUC Lua  [id: 42][name: HeloWorld Driver][file: HeloWorld.c4z]`


Condition : A Lua driver loaded successfully with PUC Lua:

Sample Entry : `2018-10-10 12:34:56.789 -0600 ea5-000DEADBEEF [1234] DEBUG: Lua driver loaded successfully with PUC Lua [id: 42][name: HeloWorld Driver][file: HeloWorld.c4z]`


Condition : A Lua driver loaded successfully with PUC Lua, but there are runtime errors:

Sample Entry : `2018-10-10 12:34:56.789 -0600 ea5-000DEADBEEF [1234] ERROR: Lua driver loaded with PUC Lua but there were one, or more, runtime errors. This driver may not function correctly [id: 42][name: HeloWorld Driver][file: HeloWorld.c4z]`


### Driver Validation using DriverValidator

Driver Validator is a DriverWorks SDK compiled Python utility used to validate .c4z files. This section will explain how to execute the utility from a command line to validate a driver's LuaJit compatibility. 

An example of executing drivervalidator against a driver called HelloWorld.c4z would be:

`-d "C:\\Users\username\Documents\Control4\Drivers\Hello World\Hello World.c4z" -v 1 -1 8`

In the example above, the -d is required followed by the path to the .c4z file that will be validated. The verbosity level (-v 1) is set with a value of 1 or 2. A value of 1 is least and 2 is most. The example above shows a verbosity level of least. In the example above (-v 1 -1 8) the value of 8 is passed for a logging level of DEBUG. Logging level values are passed as follows:

`WARN = 1`
`FATAL = 2`
`FAIL = 3`
`ERROR = 4`
`PASS = 5`
`INFO = 6`
`NOT_IMPLEMENTED -= 7`
`DEBUG = 8`

Selecting a logging level will include all log entries for it and any entries for logging levels lower than it. For example, selecting a log level of ERROR will log all Error entries as well as FAIL, FATAL and WARN. The NOT IMPLEMENTED log level is has been included for driver certification testing purposes. This log level is useful in identifying instances where a test routines have been created for driver code, but the driver being validated has not implemented that code.


