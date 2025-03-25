
## Camera Test in Composer Pro

The Camera Test screen in Composer Pro provides a convenient way for dealers to test a camera’s connection/configuration using test URLs supported by the camera. Testing includes connection and display of:

- Snapshot URLs
- Motion JPEG URLs
- Video URLS
- Streaming URLs _(new in O.S. 4.0.0)_
			 
<img src="images/camimage3.png"/>


In order for a camera driver to take advantage of the test screen, the driver needs to supply all of the test URLs that the camera offers for testing purposes. 
 
Using the Camera Test screen is the same for all four test scenarios. For example, when a dealer clicks on the Get Steaming URLs button, the [GET\_STREAMs\_URLS][1] Proxy Command is sent to the camera driver. The driver  needs to handle this command and return the desired URLs to populate the test list on the Camera Test screen. When reviewing the Get\_STREAMS\_URLS command you’ll see that codec, resolution and frames per second data can be also be sent with the URLs. Currently, testing of any h265 codec is not supported in Composer Pro.

After the driver delivers the desired URL data, the dealer can select a stream  click on the Test button. 

<img src="images/camimage4.png"/>


The test result is displayed at the bottom, They include both success and fail.

The Proxy Command sent for the Snapshot URLs test is [GET\_SNAPSHOT\_QUERY\_STRING][2]

The Proxy Command sent for the Motion JPEG URLs test is [GET\_MJPEG\_QUERY][3].

The Proxy Command sent for the Video URLs test is [GET\_RTSP\_H264\_QUERY\_STRING][4]

[1]:	https://snap-one.github.io/docs-driverworks-proxyprotocol-camera-4.0.0-beta/#camera-proxy-commands-get_stream_urls
[2]:	https://snap-one.github.io/docs-driverworks-proxyprotocol-camera-4.0.0-beta/#camera-proxy-commands-get_snapshot_query_string
[3]:	https://snap-one.github.io/docs-driverworks-proxyprotocol-camera-4.0.0-beta/#camera-proxy-commands-get_mjpeg_query
[4]:	https://snap-one.github.io/docs-driverworks-proxyprotocol-camera-4.0.0-beta/#camera-proxy-commands-get_-rtsp_h264_query_-string