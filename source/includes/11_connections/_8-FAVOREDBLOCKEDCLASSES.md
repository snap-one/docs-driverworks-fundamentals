
# Favored and Blocked AV Connection Classes

Two AV connection class-level driver elements have been delivered in conjunction with O.S 3.2.0: Favored classes and Blocked classes:

- `<favoredclasses></favoredclasses>`

- `<blockedclasses></blockedclasses>`


These properties provide the ability to designate an AV connection class or classes as “favored” or “blocked”. 

Favoriting or blocking connections in your connection XML code can be a helpful solution that is most often needed in AV Receiver drivers. Specifically, when handling audio signals distributed across multiple zones where signal conversion is not handled consistently by the device (i.e., receiver). Signal conversion (i.e., HDMI input to analog stereo output) is defined in Control4 drivers via the down-classing and up-classing capabilities.


### A Use Case for Favored Connection Classes
To better understand the problem that favoriting connection classes can resolve, let’s consider the following AV Receiver which resides in a Control4 project: 


- The ability to down-class digital audio (HDMI) for the receiver’s Zone 1 (STEREO / SPEAKER).

- The inability to down-class digital audio (HDMI) for the receiver’s Zone 2 (STEREO / SPEAKER).

- Media Player 1 bound via HDMI and STEREO

- Media Player 2 bound via HDMI

The goals of successful pathing architecture is to minimize the switch of audio class within a path while not consuming higher quality components with a lower quality stream. This approach leaves those components available to use a higher quality stream.

So, when audio is selected for Zone 1 through the Control4 UI, Director will select Media Player 1 via the STEREO audio path. The result is audio successfully being played in the receiver’s Zone 1 through the STEREO connection. Once HDMI has been downmixed to STEREO, it will attempt to stay on STEREO.

Next, while Zone 1 is still playing audio, the user selects Media Player 2 in Zone 2 to play another audio signal (since Media Player 1 is already in use in Zone 1). Director’s pathing architecture has no way of knowing that the receiver lacks the ability to down-class HDMI to STEREO on Zone 2 so it presents Media Player 2 as an available source. This results in Media Player 2 being selected in Zone 2 UI but no audio being distributed due to it being an HDMI source signal.

This example was selected for simplicity. A more typical example is when multiple outputs of a Control4 controller are bound to an AV Matrix and then the AV Matrix is bound to a Receiver for a few zones. In this example, the user(s) are selecting audio streams from sources such as Tidal and TuneIn so the user has no idea what output Director may be selecting. 

Blocking and favoring AV connection classes give the driver writer the ability to influence the choices Director’s pathing architecture makes to successfully connect inputs to outputs and avoid the scenarios described above in Zone 2.  This is accomplished by applying the favored classes XML elements within the output connections in the driver’s Connection XML code. An example of the XML needed to favor a class is to the right.

```xml
-- Favored Example

<favoredclasses>
  <classname>HDMI</classname>
  <classname>STEREO</classname>
</favoredclasses>
```

In the use case defined above, we can resolve the lack of audio availability in Zone 2 by using favored classes. But to do so, we need to start with Zone 1. We’re going to favor some of the other connection classes the receiver supports in Zone 1. By favoriting these classes, Director will select from those classes to play the audio. This frees up connection classes we know can be can successfully used for playing audio in Zone 2. In the example to the right you can see the connection XML for the Main Output of the receiver or Zone 1. 

```xml
-- Main Zone Example

<connection proxybindingid="5001">
            <id>4000</id>
            <facing>1</facing>
            <connectionname>Main Output</connectionname>
            <type>6</type>
            <consumer>False</consumer>
            <audiosource>True</audiosource>
            <videosource>False</videosource>
            <linelevel>True</linelevel>
            <classes>
                <class>
                    <classname>SPEAKER</classname>
                </class>
                <class>
                    <classname>STEREO</classname>
                </class>
            </classes>
            <favoredclasses>
                 <classname>HDMI</classname>
                 <classname>DIGITAL_COAX</classname>
                 <classname>DIGITAL_OPTICAL</classname>
            </favoredclasses>
</connection>
```


Note that the following connection classes are favored using the new  `<favoredclasses></favoredclasses>` XML element: 

- HDMI
- DIGITAL\_COAX
- DIGITAL\_OPTICAL

Beginning with O.S release 3.2.0, Director will select a favored connection class first. In this example, by not favoring SPEAKER and STEREO there is a higher probability that they will be available to the other Zones, such as Zone 2 that require this type of signal. 

At this point in the use case, we’ve favored the signal classes in our main zone (Zone 1). In doing so we’ve increased the probability that they will be selected in Zone 1.  This leaves the lower classes (STEREO and SPEAKER) available for Zone 2. However, there are some scenarios where the STEREO signal may be in use and not available to the Zone 2. To prevent the selection of the source being a digital class in Zone 2, we must block those signals in the Zone 2 connection XML to be sure that they cannot be selected. We can see in the Zone 2 connection example to the right that we’ve blocked: 

- HDMI
- DIGITAL\_COAX
- DIGITAL\_OPTICAL

```xml
-- Zone 2 Example

<connection proxybindingid="5001">
            <id>4002</id>
            <facing>1</facing>
            <connectionname>Zone2 Output</connectionname>
            <type>6</type>
            <consumer>False</consumer>
            <audiosource>True</audiosource>
            <videosource>False</videosource>
            <linelevel>True</linelevel>
            <classes>
                <class>
                    <classname>STEREO</classname>
                </class>
            </classes>
            <blockedclasses>
                 <classname>HDMI</classname>
       	 	<classname>DIGITAL_COAX</classname>
                 <classname>DIGITAL_OPTICAL</classname>
            </blockedclasses>
</connection>
```


By using favored connection classes in Zone 1 and blocked connection classes in Zone 2 we’re putting the AV Receiver driver in a position to influence Director to successfully distribute audio signals to both Zones regardless of the Signal Conversion limitations of the device.
