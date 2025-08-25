
## JumpRefresh Utility

JumpRefresh is a program that is a companion tool to JumpStart. While JumpStart will create source code for a new driver, JumpRefresh only copies template files that are not intended to be changed by the driver developer into the driver’s source code. The allows the driver developer to update their driver to use the latest template code without overwriting any changes or additions made to the driver code after its initial creation.

The program takes the name of a .jumprefresh file as a parameter. The file is a Json file that contains:

1. Information about where the template code is located 
2. Information about where the source directory for the driver source code is located. 
3. A list of which proxy and communication modules should be updated.

The following is an example of executing JumpRefresh:

`python JumpRefresh.exe avswitch-ACME.jumprefresh`

The following is an example of .jumprefresh file contents:

```json
{"TemplateDir": 
	"developcontrol4DriverDevdriverstemplate-code",
	"DriverDir":"C:developavswitch-ACME", 
	"Proxies": ["AV Switch"],
	"Communications":["serial"]
}
```
