
The included Lua method will escape XML code to the Lua driver. Any time string data is put into XML this method must be called.

```lua
int LuaC4Object::XmlEscapeString(lua_State *L)
 {
       LuaParamType params[] ``=
       ``{
              {"string", "stringToEscape" }``,
       }``;``

        if (!CheckParams(L, params, ARRAY_SIZE(params``)
              return 0``;``

       std::string stringToEscape = lua_tostring(L, 1)``;``

       std::string strVariable = XmlEscapeMyString(stringToEscape.c_str())``;
       lua_pushlstring(L, strVariable.c_str(), strVariable.size())``;
       return 1``;`
}
```

For example:

`strXmlListItem = “<text>" .. EscapeXML(strListItem) .. "</text>”`