## Contact and Relay Bindings

 Contact and Relay bindings are also control bindings. To the right are two examples:

```xml
 <connection>
      <id>11</id>
      <facing>6</facing>
      <connectionname>Security Zone 1</connectionname>
      <type>1</type>
      <consumer>False</consumer>
      <audiosource>False</audiosource>
      <videosource>False</videosource>
      <linelevel>False</linelevel>
      <classes>
        <class>
          <classname>CONTACT_SENSOR</classname>
        </class>
      </classes>
</connection>

and

<connection>
      <id>24</id>
      <facing>6</facing>
      <connectionname>Relay Port 1</connectionname>
      <type>1</type>
      <consumer>False</consumer>
      <audiosource>False</audiosource>
      <videosource>False</videosource>
      <linelevel>True</linelevel>
      <classes>
         <class>
            <classname>RELAY</classname>
         </class>
      </classes>
 </connection>
```

Remember, you can always refer to existing drivers for the device type that you are creating to see how the \<connections\> XML section should be defined.

