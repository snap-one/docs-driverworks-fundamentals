
## History Event Notifications

**Overview**

Beginning with O.S. 3.4.0, drivers can provide a better experience for users receiving Event push notifications on smartphones or tablets. Users can now view these notifications and, if desired, select the notification and be taken directly to the relevant content within the Control4 app. This removes the previous need to navigate through the app to find the relevant information. 

For example, consider an IP Camera sending a push notification from a Motion event type. The notification displayed on the UI can be selected and the user is taken into the camera area of the app and the video that triggered the event. If the event was triggered in the past, the notification link will take the user to the History UI.

Prior to O.S. 3.4.0, driver developers had to use the SendUIRequest API to directly send the RECORD\_HISTORICAL\_EVENT command to the agent in order to get back the UUID of the event. This functionality has also been enhanced through an updated Record History API. 


**Role of the Navigation Agent**
In order for the deep linking to work, the Navigation agent must be installed. This is where the URI is generated. Without the navigation agent installed, the director.log will look similar to the example to the right.

```lua
2023-06-01 13:34:05.964 -0600 core3-000FFF0C34A4 [2330] (2244) DEBUG: HistoryAgent::getBuildHistoryURI: Started - Action: Aquiring Navigation Agent Device Id.

2023-06-01 13:34:05.964 -0600 core3-000FFF0C34A4 [2330] (2244) ERROR: HistoryAgent::getBuildHistoryURI: Failed - No Navigation Agent found using control4_agent_navigation.c4i.
```

Once the Navigation Agent is successfully installed, the director.log will be similar to the example to the right. Note the inclusion of the URI.

```lua
2023-06-01 13:39:39.479 -0600 core3-000FFF0C34A4 [2330] (2244) DEBUG: HistoryAgent::getBuildHistoryURI: Started - Action: Aquiring Navigation Agent Device Id.
2023-06-01 13:39:39.480 -0600 core3-000FFF0C34A4 [2330] (2244) DEBUG: HistoryAgent::getBuildHistoryURI: Response - XML: <uri>/v1/rooms/268/cameras/827</uri>
2023-06-01 13:39:39.480 -0600 core3-000FFF0C34A4 [2330] (2244) DEBUG: HistoryAgent::getBuildHistoryURI: Response - URI: '/v1/rooms/268/cameras/827'
```


