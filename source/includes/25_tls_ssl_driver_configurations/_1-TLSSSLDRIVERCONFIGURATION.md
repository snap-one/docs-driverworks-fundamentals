## TLS/SSL Driver Configuration

Control4 drivers can connect securely with devices using SSL/TLS. These connections are outgoing TCP client connections, using either the C4:url calls or the standard network connections (either static in XML or dynamic in Lua).

If you use the DriverWorks SDK URL interface, you can implement basic or digest authentication support within your driver and construct the url properly to include those credentials in communication with the device. For more information, please see:

[https://snap-one.github.io/docs-driverworks-api/#url-interface][1]
[https://snap-one.github.io/docs-driverworks-api/#netportoptions][2]
[https://github.com/control4/docs-driverworks/tree/master/sample\_drivers][3]

A “class” of connections that enable declaring secure (SSL) connections in a driver file (.c4z) is defined to the right. As part of this, the “port” section supports some additional properties that enable various features of SSL. To the right is an example taken from the HC-800 driver file.
 

```xml
<classname>SSL</classname>
    <ports>
       <port>
          <name>Sysman</name>
           <number>5810</number>
           <auto_connect>True</auto_connect>
           <monitor_connection>True</monitor_connection>
           <keep_connection>True</keep_connection>
           <keep_alive>True</keep_alive>
           <delimiter>4f4b0a</delimiter>
       </port>
    </ports>
```
 

This snipped declares an SSL connection bound to port 5810 (Sysman). This particular connection doesn't require the use of any special properties. To the right is another example that illustrates the five new properties that were added for SSL.
 

```xml
<port>
  <name>CertTest</name>
  <number>6666</number>
  <auto_connect>True</auto_connect>
  <monitor_connection>True</monitor_connection>
  <keep_connection>True</keep_connection>
  <keep_alive>True</keep_alive>
  <delimiter>4f4b0a</delimiter>
  <certificate>./protected.pem</certificate>
  <private_key protected="True">./protected.pem</private_key>
  <cacert>./cacert.pem</cacert>
  <verify_mode>peer</verify_mode>
  <method>sslv3</method>
</port>
```
 

The properties are defined as:

`certificate` - Path to the certificate to use for the connection. The path is relative to driver’s C4Z location.


`private**_key`- Path to the private key to use for the connection. The path is relative to the driver’s C4Z location. If the “protected” attribute is “True”, then Director will invoke the callback shown to the right to retrieve the password from the driver:

```xml
function GetPrivateKeyPssword(Binding, Port)
  return “TheKey”
end
```

See below for GetPrivateKeyPassword definition.

`cacert` - Path to the CA (certificate authority) certificate to use for the connection. The path is relative to the driver’s C4Z location.


`verify_mode` - Specifies the verification mode to use for the connection. The verify mode corresponds to those supported by OpenSSL. A detailed description of the verification modes can be found here:


 `https://www.openssl.org/docs/ssl/SSL_CTX_set_verify.html. `\_ 
Note that Control4 currently supports only the peer verification mode (`SSL_VERIFY_PEER`). Value values include:

- `none`
- `peer`


If this property is omitted, then Director defaults to use no verification (“none”).


`method` - Specifies the method to use for establishing the connection. A detailed description of the various methods can be found here: `https://www.openssl.org/docs/ssl/SSL_CTX_new.html`. Valid values include:


- `sslv2`
- `sslv23`
- `sslv3`
- `tlsv1`
- `tlsv1_1`

If this property is omitted, then Director defaults to using sslv23 (which is the OpenSSL default).


Please note that the `<certificate>`, `<private_key>`, and `<cacert>` expected in those XML tags for certificates and key information may all be contained in the same file.  It's not required to split the various certificates and keys out into separate files to work properly.  In the case that they are all contained in a single file, put that file's filename value in each of the XML tags.


If a private key / key exchange is desired, you need to include your client\_private.key within the c4z file that contains your driver code. It should be pre-encrypted to protect the private key. You can then use the [https://snap-one.github.io/docs-driverworks-api/#getprivatekeypassword][4] in your lua code to return the password that is needed for the Control4 system to access and use the private key in the key exchange. Obviously you will want to encrypt your driver code before distributing it in order to protect the client private key.

Once the connection is established, then you simply utilize the standard networking functions (e.g. [NetConnect][5] ) to communicate with the device. The returned information will come back on [ReceivedFromNetwork][6]

Your driver then will be responsible for parsing the JSON data structure and handling it as you deem necessary.

If you want to use a web socket for your device communication, we have a sample web socket driver in our SDK library at:
[https://github.com/control4/docs-driverworks/tree/master/sample\_drivers/websocket][7]

You can test using standard web socket and web socket secure (TLS). You can use the websocket.org demo server to test regular and secure WebSockets:

[ws://echo.websocket.org][8]

[wss://echo.websocket.org][9]

[1]:	https://snap-one.github.io/docs-driverworks-api/#url-interface
[2]:	https://snap-one.github.io/docs-driverworks-api/#netportoptions
[3]:	https://github.com/control4/docs-driverworks/tree/master/sample_drivers
[4]:	https://snap-one.github.io/docs-driverworks-api/#getprivatekeypassword "GetPrivatekeyPassword API"
[5]:	https://snap-one.github.io/docs-driverworks-api/#netconnect
[6]:	https://snap-one.github.io/docs-driverworks-api/#receivedfromnetwork
[7]:	https://github.com/control4/docs-driverworks/tree/master/sample_drivers/websocket
[8]:	ws://echo.websocket.org
[9]:	wss://echo.websocket.org