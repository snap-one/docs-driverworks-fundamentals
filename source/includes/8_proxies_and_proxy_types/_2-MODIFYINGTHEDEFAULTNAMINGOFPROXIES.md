## Modifying the Default Naming of Proxies

When a device is added to a project within Composer, the name of the device displayed in the project is inherited from the device proxy. It is possible modify the name for a proxy that will be displayed in composer. This is especially useful when a proxy includes several “devices” . The name can be changed in the proxies section of the driver. In the example below, a thermostat has been renamed. Instead of the device being identified in composer as “Thermostat” it will now be displayed as “Carrier Infinity Thermostat”.

```xml
<proxies qty="1">
  <proxy proxybindingid="5001" name="Carrier Infinity Thermostat">~thermostat</proxy>
</proxies>
```
