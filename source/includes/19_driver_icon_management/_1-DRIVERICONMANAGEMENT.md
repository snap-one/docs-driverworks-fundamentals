## Driver Icon Management

**Note: Detailed information regarding the creation of Device and Service Icons as well as useful Icon Templates can be found in the DriverWorks SDK at: DriverWorks SDK/Icon Templates.**


### Managing Driver Icons

Drivers are graphically represented within the ComposerPro environment using icons. These icons are displayed in several places: 

The System design view found on the left hand side of Composer Pro:

todo img 1


The List View area in the center of the ComposerPro window:

todo img 2


Under the My Drivers tab found in the Items area on the right hand side of the ComposerPro window:

todo img 3


Before discussing how these icons are included in drivers, it's important to note the differences in icon sizes in the images above. The System Design and Items areas accept an icon that is 16 X 16 pixels in size. This is referred in driver development terminology as the "small image" or `small_icon` in XML. The List View area uses a larger image at a size of 32 X 32 pixels. This image is referred to as the "large image" or `large_icon` in the driver's XML:

Small Image					Large Image
todo img 4

16 X 16                                                  32 X 32

Driver icons are defined in the driver's XML. The remainder of this document will identify several types of drivers and, depending on where the driver's icons are located, outline the driver attributes required to display the icons in ComposerPro.


### Combo Drivers
Combo Drivers are drivers that represent both the proxy and protocol information in a single driver file. They are also the simplest with regard to implementing driver icons. The example below shows a combo driver's XML and identifies the attributes that define the driver's icons: the small and large XML tags:

```xml
<created></created
<modified></modified>
<version></version>
<small></small>
<large></large>
<control></control>
<controlmethod></controlmethod>
<combo></combo>
```

The contents of these tags are used to identify the small and large versions of the driver's icons. Control4 delivers numerous icons, which are available when ComposerPro is installed. For example, in a typical installation these icons can be found at: C:/Program Files/Control4/ComposerPro280/Images

Here you will find folders for both large and small icons. If you would like to use one of the included icons for your driver, the icon name will need to be included in the combo driver's XML tags. For example, the large icon name for the Portable V2 Touchscreen is `control4_touchpad_32` and the small icon name is `control4_touchpad_16`. To use these icons the combo driver's XML will need to be updated to:

`<small>devices_sm/control4_touchpad_16.gif</small>`

`<large>devices_lg/control4_touchpad_32.gif</large>`

Next, let's consider an example of a combo driver that requires icons not provided by Control4. This driver obtains its small and large icons from a URL. The same XML tags can be used but instead of an icon name, a URL to the image is inserted. For example:

`<small>http://root/images/small/small_icon.gif</small>`

`<large>http://root/images/large/large_icon.gif</large>`

Note the use of "http://" in both of the XML strings above. This is a pathing requirement in using icons from a URL.

If the combo driver is a .c4z driver file, and wants to provide its own image, there are two additional components to implementing icons. 

The first is an XML attribute that must be included in the small and large XML strings. The attribute is: `image_source` and it must be equal to "c4z". For example:

`<small image_source="c4z">images/devices_sm/small_icon.gif</small>`

`<large image_source="c4z">images/devices_lg/large_icon.gif</large>`

The second component required in .c4z files is the creation of a directory at the root level of the .c4z file itself. This directory will contain a path to the small and large icons you wish to use for your driver. Defining the directories levels below the root level www directory are up to the driver developer. For our example, we'll use a path of:

`www/images/devices_sm`

and

`www/images/devices_lg`

The `devices_sm` directory will contain the small images and the `devices_lg` will hold the large format.  Using the example above we can see the directory path (following the www level) to the images:

`<small image_source="c4z">images/devices_sm/small_icon.gif</small>`

`<large image_source="c4z">images/devices_lg/large_icon.gif</large>`


### Single Proxy Drivers
Single Proxy drivers define their icons using the same XML strings as a combo driver. However, they also have the ability to define additional icons in the \<proxies\> area of the XML.

If the \<small\>\</small\> XML tag is populated with an image or UL to an image, that file will always be used in the My Drivers list of the Items area in ComposerPro. The \<large\>\</large\> XML tag is used to populate the List View area of ComposerPro unless a different icon is defined in the \<proxies\>\</proxies\> XML section of the driver. If a different icon is defined, the contents of the \<large\>\</large\> XML tag will be disregarded.

Additional icons and other useful attributes are defined under the \<proxies\> XML tag. For example, let's consider a Keypad driver that obtains its icon from the delivered images with ComposerPro. It's proxy XML definition looks like this:

```xml
<proxies qty="1">~
<proxy proxybindingid="5003" name="KEYPAD" small_image="devices_sm/keypad_16.gif" large_image="devices_lg/keypad_32.gif">light_v2</proxy>
```


If we look at the XML attributes for the proxy section we can see the proxy binding ID is defined first. This is required for the correct icon to be displayed. Next we see a name attribute. This attribute is useful as it defines the driver's name that will be displayed in ComposerPro and on devices running Navigator. This is especially useful when dealing with multi-proxy drivers and its use is further explained below. When left blank, the name of the proxy used by the driver will be displayed. Following the name attribute we can see the small and large image icon attributes. Note the format the XML follows:

`small_image="devices_sm/keypad_16.gif"`

`large_image="devices_lg/keypad_32.gif"`
\`
\`
In the example above, we can see the `small_image` and `large_image` directories that are installed with ComposerPro as well as the small and large icon names within each`.`

If our single proxy driver requires the use of icons through a URL, the proxy XML would look like this:\`\` \`\`

```xml
<proxies qty="1">
 <proxy proxybindingid="5001" name="KEYPAD" image_source="url" 
  small_image="http://192.168.1.111/driver/`driver_singleproxy/image/devices_sm/keypad_16.gif" 
  large_image="http://192.168.1.111/driver/`driver_singleproxy/image/devices_lg keypad_32.gif">keypad_proxy</proxy>
</proxies>
```


Again we see the proxy binding ID and name attributes defined followed by the `image_source` value. This is followed by the small and large image attributes but note the URL to the images in quotations. As mentioned earlier, the only requirement for the URL definition is that it begins with "http://`"`

Finally, if our single proxy driver uses an image provided through the .c4z file, our \<proxy\> XML would look like this:

```xml
<proxies qty = "1">
<proxy proxybindingid="5002" name="KEYPAD" image\_source="c4z" small\_image="images/devices\_sm/keypad\_16.gif"\` 	large_image="Images/Devices_Lg/keypad_32.gif">light_v2</proxy>
</proxies>
```

In the example above, we see an attribute that is required for .c4z use: `image_source`. As mentioned earlier, this attribute must be included and it must be equal to "c4z". For the small and large image attribute we see a directory path. This path is found below the required www directory at the root level of the .c4z file. 

The www directory contains a path to the small and large icons you wish to use for your driver. Defining the directories levels below the root level www directory are left up to the driver developer. For our example, we'll use a path of:

`www/images/devices_sm`

and

`www/images/devices_lg`

In this driver, the `devices_sm` directory will hold the small icons and the `devices_lg` will hold the large format.  Using the example above we can see the directory path (following the www level) to the images.

Note that if no custom images are defined in the driver's XML, the default icon for the Control4 Proxy used by the driver will be displayed.


### Multi Proxy Drivers
Icons are handled in multi-proxy drivers in much the same way as single proxy drivers. The XML differs only slightly in that additional proxy tags are included for each proxy used by the driver. For this example, we'll use XML for a receiver driver that uses the Receiver Proxy as its primary proxy. The driver also has two DVD sub-proxies, which are used to control a media player and a media streaming device. 

This driver implements icons from two different locations: the receiver portion of the driver gets its icons from Control4's Receiver Proxy, the media player and the streaming device get their icons from a .c4z file. The XML for the driver looks like this:

```xml
<small image_source="c4z">images/devices_sm/receiver_16.gif</small>
<large image_source="c4z">images/devices_lg/receiver_32.gif</large>
<control>lua_gen</control>
<controlmethod>~serial~</controlmethod>
<driver>~DriverWorks~</driver>
<proxies qty="3">
<proxy proxybindingid="5001" name="My Receiver">~receiver~</proxy>
<proxy proxybindingid="5002" name="Media Player" image_source="c4z" small_image="images/devices_sm/media_player_16.gif” 
large_image="images/devices_lg/media_player_32.gif">dvd</proxy>
<proxy proxybindingid="5003" name="Streaming" image_source="c4z" small_image="images/devices_sm/stream_16.gif"large\_image="i`mages/devices\_lg/stream\_32.gif">dvd~</proxy>
</proxies>
```

It's worth noting several attributes in the XML example to the right. First, the \<proxies qty\> tag has a value equal to 3, the amount of proxies this driver uses: one Receiver and two DVD proxies. Note that each of the individual proxies uses their own proxy binding IDs: 5001, 5002 and 5003. Proxy Binding IDs are required each so the correct icons are displayed. Also, this example shows the usefulness of the name= attribute. Each of the proxies have a unique name. This is especially useful with the multiple instances of DVD sub-proxies. In addition to unique driver icons, the use of "Media Player" and "Streaming" for the name attributes will make it easier to distinguish between the two in ComposerPro and Navigators:

todo img 6
