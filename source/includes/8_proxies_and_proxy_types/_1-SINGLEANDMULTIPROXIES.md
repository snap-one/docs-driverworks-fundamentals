## Single and Multi-Proxies

Most devices will require the use of only a single proxy.  Be sure to choose the correct proxy type for your device.  When using a single proxy, your .c4z can specify the proxy using a simple declaration in the .c4i file. For example:

`<proxy>discchanger</proxy>`

or

`<proxy>TV</proxy>`



If you decide to not use an existing proxy and create one; you should specify the proxy with the same name as the .c4z filename.  Your driver will not show up for selection within Composer if your proxy name is not a valid proxy type for the Control4 system and doesn’t match your .c4i filename.

Alternatively, even for a single proxy driver, you may choose to use the more complete declaration as in the first two examples to the right:

```xml
<proxies>
  <proxy proxybindingid="5001">discchanger</proxy>
</proxies>
```



```xml
<proxies>
 <proxy proxybindingid="5001">~tv~</proxy>
</proxies>
```

This is also the format used with multi-proxy devices as seen in the remaining examples:

```xml
<proxies>
  <proxy proxybindingid="5001">tuner</proxy>
  <proxy proxybindingid="5002">tuner</proxy>
  <proxy proxybindingid="5003">tunerXM</proxy>
</proxies>
```


```xml
<proxies>
   <proxy proxybindingid="5001">receiver</proxy>
   <proxy proxybindingid="5002">tuner</proxy>
   <proxy proxybindingid="5003">tunerXM</proxy>
   <proxy proxybindingid="5004">dvd</proxy>
   <proxy proxybindingid="5005">tunerXM</proxy>
 </proxies>
```

Each proxy within a driver is given a binding ID in the \<connections\> section of the .c4z.  If you have multiple proxies of the same type you’ll need to explicitly state the proxybinding id for each proxy.  The proxy binding ID is included in every command received from the system for your DriverWorks driver.

If you have any questions about how to define the proxy or proxies for a specific device type, you might find it helpful to refer to an existing driver.  Proxies are defined in the same way for DriverWorks and for non-DriverWorks drivers.