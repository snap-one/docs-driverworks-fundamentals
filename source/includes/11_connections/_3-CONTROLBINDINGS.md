## Control Bindings

The serial port’s communication parameters are defined in the \<config\> or the \<capabilities\> section of the driver using as shown to the right:

```xml
<connection>
      <id>1</id>
      <facing>6</facing>
      <connectionname>Serial RS-232</connectionname>
      <type>1</type>
      <consumer>True</consumer>
      <audiosource>False</audiosource>
      <videosource>False</videosource>
      <linelevel>False</linelevel>
      <classes>
        <class>
          <classname>RS_232</classname>
        </class>
      </classes>
</connection>
```

The serial port’s communication parameters are defined in the \<config\> or the \<capabilities\> section of the .c4z using:

`<serialsettings>9600 8 odd 1 none</serialsettings`

The statement above defines the following communication parameters:

- 9600 baud
- 8 data bits
- odd parity
- 1 stop bit
- no flow control


The serial port binding is also a control binding

```xml
<connection>
      <id>1</id>
      <facing>6</facing>
      <connectionname>Serial RS-232</connectionname>
      <type>1</type>
      <consumer>True</consumer>
      <audiosource>False</audiosource>
      <videosource>False</videosource>
      <linelevel>False</linelevel>
      <classes>
        <class>
          <classname>RS_232</classname>
        </class>
      </classes>
</connection>
```
