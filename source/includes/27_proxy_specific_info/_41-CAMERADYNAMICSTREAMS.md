
## Camera Proxy and Dynamic Streams

The Camera Proxy has been enhanced with regard to what types of streams it provides. This is a result of the release of Control4’s next generation T4 touchscreens, new controllers and support for H.265 streaming. Camera drivers now have the ability to provide more stream options and can support a larger set of mixed hardware environments.

Also, Navigators now support streaming tiles which results in the need to stream many cameras in parallel. To provide this functionality over a remote connection, it is desirable to offer the option to pick a lower bandwidth stream.

For the best experience, cameras should be minimally configured with the following streams by default:

| Next Gen C4 OSD       | T4 Touch Screens | Mobile                |
| --------------------- | ---------------- | --------------------- |
| High Resolution H.265 | 1080p H.265      | High Resolution H.265 |

| Legacy C4 OSD | T3 Touch Screens |
| ------------- | ---------------- |
| 720p H.264    | 720p H.264       |

| Small Streaming Tiles |
| --------------------- |
| Low Resolution H.265  |


#### Driver Support for Dynamic Streams

New camera drivers should implement the dynamic streams API to provide a list of streams that the Navigators can pick from. In order to support dynamic streams the driver should:

1. Set the requires\_dynamic\_stream\_urls dynamic capability to “true”.

2. Implement the GET\_STREAM\_URLS UI request command. This can be either implemented synchronously, by returning the list of cameras directly or it can also be implemented asynchronously. For example, if the driver needs to query a web service to get the current streams.

If implemented asynchronously, when the stream list is available it will be necessary to send the Proxy the STREAM\_URLS\_READY Notify with the results. 

Drivers that do not have static URLs for streams will need to respond to the GET\_STREAM\_URLS call with an XML object that indicates the URL(s) is/are being generated and include a key with the response.  Once the driver has been able to obtain a URL for the stream, the driver should send a STREAM\_URLS\_READY Notify with the same key that is provided with this Synchronous call response.  An example of the response is:

`<streams generating_key="1234"/>`

The key should be a unique 32 bit integer value.  We recommend starting at 1 and incrementing by 1 for each request that comes in.

Also, if the driver does have a valid temporary URL that is still valid when it gets the call, no further Notify is needed and the driver should respond to this call with the XML of streams information.

To indicate to a client that the driver supports this API call, a driver needs to set the requires\_dynamic\_stream\_urls capability to true in the driver c4i or via DYNAMIC\_CAPABILITIES\_CHANGED Notify.

If no Port is provided with a URL, the following ports are used by all clients as the default for the transport:

- http - 80
- https - 443
- rtsp - 554
- rtsps - 443


#### Support for Caching  URLs

The ability to cache the stream URL list is also supported. This removes the need to have to execute the GET\_STREAM\_URLS request every time a camera stream is loaded. This is supported with the capability: dynamic\_urls\_use\_type. 

The driver also needs to send a DYNAMIC\_URLS\_CHANGED notification to the proxy when the configured URLs have been changed. The proxy would then send a "DYNAMIC\_URLS\_CHANGED" DataToUI in response to this that the Navigators would key off of to refresh any cached URLs. This is useful for driver's with MULTIPLE use type URLs.



### Related APIs

### requires\_dynamic\_stream\_urls

Capability determining whether the camera should be asked for stream URLs through the more advanced API call.


### Signature

`<requires_dynamic_stream_urls></requires_dynamic_stream_urls>`

### Type

Boolean: Defaults to false.

### Dynamic Capability

Yes

### Example

```xml
<capabilities>
    <requires_dynamic_stream_urls>true</requires_dynamic_stream_urls>
</capabilities>
```


### requires\_dynamic\_snapshot\_urls

Capability determining whether the camera should be asked for snapshot URLs through the more advanced API call.


### Signature

`<requires_dynamic_snapshot_urls></requires_dynamic_snapshot_urls>`

### Type

Boolean: Defaults to false.

### Dynamic Capability

Yes

### Example

```xml
<capabilities>
    <requires_dynamic_snapshot_urls>true</requires_dynamic_snapshot_urls>
</capabilities>
```


### GET\_STREAM\_URLS

This command is used to obtain URLs for a camera that does not have a constant URL like a Cloud based camera or for cameras that have a wider range of features and configurable options that can produce better customer experiences using various clients. 

A Client will make this request to a driver and provide information about how the client will be using the stream.  The driver optionally can adjust the URL(s) returned based on this information.  Driver developers are encouraged to test their driver on various devices to ensure good customer experiences when viewing collages, high resolution or high frame rate streams.


| Parameter  | Description                                                                                                                                                                                                                                                                                |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CODEC      | Optional. Comma separated list of codecs that the client can support with the most preferred first.  A driver is not required to handle this but some drivers may want to if they support multiple codecs. (mjpeg, h264, h264i, h265)                                                      |
| RESOLUTION | Optional. Resolution that the client will be using to display the stream.  A driver is not required to support this but this can be helpful for clients that may need a lower resolution stream for collages of multiple cameras and a 4MP or higher stream would tax the client hardware. |
| FPS        | Optional. Frames Per Second that the client would prefer.  A driver is not required to support this but if it does it may result in better customer experience when dealing with collages on Navigators.                                                                                   |


### Examples

An example of the simplest XML returned by a driver is shown to the right:

```xml
--- simple

<streams>
   <stream url="http://192.168.1.150/view_camera.cgi">
</streams>

or

<streams>
   <stream url="https://192.168.1.150:443/view_camera.cgi" codec="h264">
   <stream url="http://192.168.1.150:80/view_camera_h265.cgi" codec="h265">
</streams>
```


This is followed by a more complex example from a camera with more features and can provide more options to allow clients to pick a better stream:


```xml
--- complex

<streams>
  <stream url="rtsps://server.company.com:554/rtsp.cgi" codec="h265" resolution="1920x1080" fps="60">
  <stream url="rtsps://server.company.com:554/cloudapi.cgi?fh2f5hw2" codec="h265" resolution="640x480" fps="5">
  <stream url="rtsp://server.company.com:554/cloudapi.cgi?fh2f4hw1" codec="h264" resolution="1920x1080" fps="60">
  <stream url="rtsp://server.company.com:554/cloudapi.cgi?fh2f4hw2" codec="h264" resolution="640x480" fps="5">
  <stream url="https://server.company.com:443/cloudapi.cgi?fh2fhsw3" codec="mjpeg" resolution="1920x1080" fps="20">
  <stream url="https://server.company.com:443/cloudapi.cgi?fh2fhsw5" codec="mjpeg">
  <stream url="http://server.company.com:80/cloudapi.cgi?fh2fhw3" codec="mjpeg" resolution="1920x1080" fps="20">
  <stream url="http://server.company.com:80/cloudapi.cgi?fh2fhw5" codec="mjpeg">
</streams>
```


### GET\_SNAPSHOT\_URLS

This command is used to obtain URLs for a camera that does not have a constant URL like a Cloud based camera or for cameras that have a wider range of features and configurable options that can produce better customer experiences using various clients. 

A Client will make this request to a driver and provide information about how the client will be using the snapshot.  The driver optionally can adjust the URL(s) returned based on this information.


### Examples

An example of the simplest XML returned by a driver is shown to the right:

```xml
<snapshots>
 <snapshot url="http://192.168.1.150/view_camera.cgi">
</snapshots>

or

<snapshots>
 <snapshot url="http://192.168.1.150:80/custom_resolution_as_requested_by_client.cgi" resolution="644x481">
 <snapshot url="https://192.168.1.150:443/view_snapshot.cgi" resolution="3840x2160">
 <snapshot url="https://192.168.1.150:443/view_snapshot.cgi" resolution="1920x1080">
 <snapshot url="https://192.168.1.150:80/view_snapshot.cgi" resolution="640x480">
</snapshots>
```

### Usage Note

**Cloud Based Cameras **

Drivers that do not have static URLs for snapshots will need to respond to this call with an XML object that indicates the URL(s) is/are being generated and include a key with the response.  Once the driver has been able to obtain a URL for the stream, the driver should send a SNAPSHOT\_URLS\_READY Notify with the same key that is provided with this Synchronous call response.  An example of the response is:

`<streams generating_key="1234"/>`

The driver would then follow up with the SNAPSHOT\_URLS\_READY Notify and specify KEY=1234 in that notify along with the URL.  The key should be a unique 32 bit integer value.  We recommend starting at 1 and incrementing by 1 for each request that comes in.

Also, if the driver does have a valid temporary that is still valid when it gets the call, no further Notify is needed and the driver should respond to this call with the XML of snapshots information.

To indicate to a client that the driver supports this API call, a driver needs to set the requires\_dynamic\_stream\_urls capability to true in the driver or via the DYNAMIC\_CAPABILITIES\_CHANGED Notify.

If no Port is provided with a URL, the following ports are used by all clients as the default for the transport:

http - 80
https - 443


### STREAM\_URLS\_READY

Protocol Notification used for following up to a GET\_STREAM\_URLS Synchronous call.  This Notify will let a UI know that the URL information they requested is ready.  The driver must use the same key parameter value that was returned to the UI as the key parameter in this notify.


| Parameter | Description                                                        |
| --------- | ------------------------------------------------------------------ |
| KEY       | Key of the originating call for the snapshot to be created.        |
| URLS      | String of XML object as described in the description of this call. |


### Examples

An example of the simplest XML used by a driver is shown to the right:

```xml
<streams key="1234">
   <stream url="http://192.168.1.150/view_camera.cgi">
</streams>

or

<streams key="1234" camera_address="192.168.1.150">
   <stream url="https://192.168.1.150:443/view_camera.cgi" codec="h264">
   <stream url="http://192.168.1.150:80/view_camera_h265.cgi" codec="h265">
</streams>
```


This is followed by an example from a camera with more features and can provide more options to allow clients to pick a better stream:

```xml
<streams key="1234" camera_address="server.company.com">
<stream url="rtsps://192.168.1.150:554/rtsp.cgi" codec="h265" resolution"1920x1080" fps="60">
<stream url="rtsps://192.168.1.150:554/cloudapi.cgi?fh2f5hw2" codec="h265" resolution"640x480" fps="5">
<stream url="rtsp://192.168.1.150:554/cloudapi.cgi?fh2f4hw1" codec="h264" resolution"1920x1080" fps="60">
<stream url="rtsp://192.168.1.150:554/cloudapi.cgi?fh2f4hw2" codec="h264" resolution"640x480" fps="5">
<stream url="https://192.168.1.150:443/cloudapi.cgi?fh2fhsw3" codec="mjpeg" resolution"1920x1080" fps="20">
<stream url="https://192.168.1.150:443/cloudapi.cgi?fh2fhsw5" codec="mjpeg">
<stream url="http://192.168.1.150:80/cloudapi.cgi?fh2fhw3" codec="mjpeg" resolution"1920x1080" fps="20">
<stream url="http://192.168.1.150:80/cloudapi.cgi?fh2fhw5" codec="mjpeg">
</streams>

```


### SNAPSHOT\_URLS\_READY

Notify used for following up to a GET\_SNAPSHOT\_URLS Synchronous call.  This Notify will let a UI know that the URL information they requested is ready.  The driver must use the same key parameter value that was returned to the UI as the key parameter in this notify.

| Parameter | Description                                                        |
| --------- | ------------------------------------------------------------------ |
| KEY       | Key of the originating call for the snapshot to be created.        |
| URLS      | String of XML object as described in the description of this call. |


### Example

```xml
<snapshots key="1234">
 <snapshot url="http://192.168.1.150/view_camera.cgi">
</snapshots>

or

<snapshots key="1234">
 <snapshot url="http://192.168.1.150:80/custom_resolution_as_requested_by_client.cgi" resolution="644x481">
 <snapshot url="https://192.168.1.150:443/view_snapshot.cgi" resolution="3840x2160">
 <snapshot url="https://192.168.1.150:443/view_snapshot.cgi" resolution="1920x1080">
 <snapshot url="https://192.168.1.150:80/view_snapshot.cgi" resolution="640x480">
</snapshots>
```


### dynamic\_urls\_use\_type

Supports the following use types:

MULTIPLE - Default if the capability is not specified. URLs can be used repeatedly. If URLs change, the driver should send DYNAMIC\_URLS\_CHANGED notification to the proxy.

SINGLE - URLs can only be used once. Driver must be asked each time for a new set of URLs.

EXPIRING (Do not include in 3.3.1) - URLs can be used more than once, but expire after some time period.


### Signature

`<dynamic_urls_use_type></dynamic_urls_use_type>`


### Dynamic Capability

Yes


### Example

```xml
<capabilities>
    <dynamic_urls_use_type>SINGLE</dynamic_urls_use_type>
</capabilities>
```


For additional Information, please see the DriverWorks Proxy and Protocol Guide regarding:

[Camera Proxy Commands][1]

[Camera Proxy Protocol Notifications ][2]

[Camera Proxy Properties][3]

[Camera Extras Interface Library][4]

[Camera Proxy Capabilities][5]

[1]:	https://jubilant-barnacle-73gylvg.pages.github.io/#camera-proxy-commands
[2]:	https://jubilant-barnacle-73gylvg.pages.github.io/#camera-protocol-notifications
[3]:	https://jubilant-barnacle-73gylvg.pages.github.io/#camera-properties
[4]:	https://jubilant-barnacle-73gylvg.pages.github.io/#camera-proxy-extras-interface-library
[5]:	https://jubilant-barnacle-73gylvg.pages.github.io/#camera-capabilities