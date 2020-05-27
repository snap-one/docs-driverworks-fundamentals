## Operating System 3 and Binary Data

With the release of OS 3, support for Lua's Just In Time compiler (LuaJIT) has been added. For more information regarding LuaJIT, please see Lua JIT Compatibility and DriverWorks Drivers content.

With regard to handling Binary Data in OS 3, Lua JIT uses a bit Library called bittop. Prior to OS 3, the PUC LUA compiler used a bit library called bitlib. To ensure that code executes identically between both compilers, the bittop Library is now being used by both LuaJIT and PUC LUA. 

While this change does not impact computations, printing to ComposerPro's Lua Output Window can be impacted if using PUC LUA's bitlib and bitwise operators for data that exceeds more than single byte operations. 

For example, to the right two outputs using bitlib:

2.10.X use of bitlib:

```js
a = bit.lshift(0xc0,24)
print(a)
3221225472
```


OS 3 use of bitlib:﻿

```js
OS3
print(bit.lshift(0xc0,24))
-1073741824
print(bit.band(bit.lshift(0xc0,24),0xffffffff))
-1073741824
a = bit.lshift(0xc0,24)
if a<0 then
a=bit.band(a,0x7fffffff) + 0x80000000
end
print(a)
3221225472
```

If PUC LUA's bitlib is used, Control4 does not recommend its use for anything more than single byte operations.


