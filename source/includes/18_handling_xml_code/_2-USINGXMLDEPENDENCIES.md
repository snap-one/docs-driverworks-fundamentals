## Using XML Dependencies to Automatically Load and Bind Dependent Drivers

Using a driver's dependencies XML, it is possible to designate other drivers as "dependent" and force those drivers to load into a project simultaneously. In addition to this, the dependencies XML can also be used to have these driver automatically bind when they enter the project.

### Use Case:

Consider an example where we'd like to add a Keypad driver to a project and then have two instances of a Light driver automatically added as well. In addition to this, we also want the connections between the Keypad driver and the two instances if the light driver created automatically as well. This can be accomplished for any set of drivers in the dependencies and connections XML area of the drivers.

First we'll look at the dependencies XML for the Keypad driver:


1. `<dependencies>`
2. `<dependency alwaysAdd="true" addMultiple="true">`
3.   `<name>light_for_keypad.c4z</name>`
4.   `<auto_bindings>`
5.   `<auto_binding>`
6.   ` <id>1</id>`
7.    `<isconsumer>false</isconsumer>`
8.    `<classes>`
9.    `<class>`
10.   `<classname>KEYPAD_LIGHT</classname>`
11.   ` </class>`
12.    `</classes>`
13.    `<dep_binding_id>14</dep_binding_id>`
14.    `</auto_binding>`
15.    `</auto_bindings>`
16.    `</dependency>`
17.  `<dependency alwaysAdd="true" addMultiple="true">`
18.   ` <name>light_for_keypad.c4z</name>`
19.    `<auto_bindings>`
20.   `<auto_binding>`
21.    `<id>2</id>`
22.    `<isconsumer>false</isconsumer>`
23.   ` <classes>`
24.    `<class>`
25.   `<classname>KEYPAD_LIGHT</classname>`
26.    `</class>`
27.   ` </classes>`
28.    `<dep_binding_id>14</dep_binding_id>`
29.    `</auto_binding>`
30.   ` </auto_bindings>`
31.    `</dependency>`
32. `</dependencies>`


Looking at lines of code 1 through 16, we can see in line 2 that the alwaysAdd tag is set to true. This means that the defined dependent driver (in this example: `light_for_keypad.c4z`) will always be added to the project when the Keypad driver is loaded. We can also see that addMultiple is also set to true. This means that more than one instance of the `light_for_keypad` driver can be added when the Keypad driver is loaded. This will allow us the two instances if the `light_for_keypad` driver when the Keypad driver loads. Line 3 clearly identifies the driver that will load with the Keypad driver.

Line 4 through 16 is the XML that will automatically create a binding between the Keypad driver and the light\_for\_keypad driver when they load into a project. The `auto_bindings` XML contains all of the Keypad driver's binding information to connect the two drivers. In Line 6 we can see the `auto_binding Id` of 1. This represents the first binding ID for the Keypad driver. That is where we want our first connection to the `light_for_keypad` driver to take place. 

Note that the Keypad driver is a Provider (isconsumer=false), this allows for multiple, dependency drivers to loaded and bound to it. We can also see in Line 10 that this binds with a classname of `KEYPAD_LIGHT`.  If we look at the `light_for_keypad` driver's connection xml, we  see that it also shares the same connection classname:

`light_for_keypad driver Connections XML`
`<connections>`
`<connection>`
 `<id>14</id>`
 `<connectionname>Keypad Light</connectionname>`
 `<type>1</type>`
 `<consumer>True</consumer>`
` <classes>`
   `<class>`
`<classname>KEYPAD_LIGHT</classname>`
   `</class>`
 `</classes>`
 `</connection>`
`</connections>`

Note that the `light_for_keypad` driver's connection XML shares the same classname as the Keypad driver:  `Keypad_Light`. We can also see in Line 6 that it is a consumer, which will make up the other half of our Provider-Consumer connection with the Keypad driver. 

Note in Line 3 that this specific connection in the `light_for_keypad` driver has an ID of 14. If you look up in the dependency XML for the Keypad driver you'll see in line 28 the `dep_binding__id `value of 14 which matches this connection in the `light_for_keypad` driver. 

Finally, if we look at the connection XML for the Keypad driver we'll see this:



1. `<connections>`
2.    ` <connection>`
3. `    <id>1</id>`
4.  `   <connectionname>Keypad Light</connectionname>`
5.  `   <type>1</type>`
6.  `   <consumer>False</consumer>`
7.  `   <classes>`
8.   `    <class>`
9.    `    <classname>KEYPAD_LIGHT</classname>`
10.   `    </class>`
11.  `   </classes>`
12. `  </connection>`
13. `  <connection>`
14.  `   <id>2</id>`
15.   `   <connectionname>Keypad Light</connectionname>`
16.   `   <type>1</type>`
17.   `   <consumer>False</consumer>`
18.   `   <classes>`
19.    `    <class>`
20. `      <classname>KEYPAD_LIGHT</classname>`
21.    `   </class>`
22.   `  </classes>`
23.   `</connection>`
24. `</connections>`
		 

Note in Line 9 that it has the same connection name as found in the `light_for_keypad` driver: Keypad Light.

The dependency XML found in the Keypad Driver and the coordinated connection XML between the Keypad driver and the `light_for_keypad` driver results in the driver being loaded when the Keypad driver loads. Also, the first binding for the Keypad driver is connected to the `light_for_keypad` driver at binding ID 14.

In the beginning of this document, we indicated that the use case calls for two instances of the `light_for_keypad` driver to load. This happens due to the second dependency binding XML found in the Keypad driver. Specifically, lines 17 through 32 represent another dependent binding that will be automatically created between the keypad driver and the `light_for_keypad` driver when they load.  

Keypad Driver Dependencies XML

17. `<dependency alwaysAdd="true" addMultiple="true">`
18. `<name>light_for_keypad.c4z</name>`
19.  `<auto_bindings>`
20.  `<auto_binding>`
21.   `<id>2</id>`
22.   `<isconsumer>false</isconsumer>`
23.   `<classes>`
24.   ` <class>`
25.  `   <classname>KEYPAD_LIGHT</classname>`
26.   `  </class>`
27.  ` </classes>`
28.  `<dep_binding_id>14</dep_binding_id>`
29.  `</auto_binding>`
30.   `</auto_bindings>`
31.   `</dependency>`
32. `</dependencies>`

Remember, this is possible because of the Keypad driver's addMultiple XML being set to true. Note that this `auto_binding ID` has a value of 2. It is the second binding point for the Keypad driver as our previous example connection used the first one. 

Note that this also has a `dep_binding_id` value of 14. This means that two connections will be made from the Keypad driver to two instances of the `light_for_key` driver. The connections will be made in each of the `light_for_keypad` drivers at binding 14. 