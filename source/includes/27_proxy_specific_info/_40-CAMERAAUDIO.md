
## Camera Proxy Audio Distribution

Beginning with O.S. 3.4.0, camera drivers have the ability to distribute audio to devices running Navigator.

If a camera supports the proper CODEC and the camera microphone has been enabled in the appropriate interface a new audio button will appear at the bottom of the single-camera view. This will allow the user to hear live audio with the live video. 

Note that if the camera does not support the proper codec, or the camera microphone has been disabled in the camera interface, no button will appear in the single-camera view.

**Requirements**

- Camera has a microphone and it's enabled. Note that most most are disabled by default. Depending on the camera/camera driver, the camera microphone can be enabled/disabled within the driver or in OvrC.


- Ensure the configuration for the camera stream is video & audio, not just video.


- The camera supports codec G.711 u-law (G.711μ).


- The driver for the camera shows the Audio Property is present.


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