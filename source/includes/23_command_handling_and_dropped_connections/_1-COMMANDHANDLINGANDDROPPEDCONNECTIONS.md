## Command Handling and Dropped Network Connections

Depending on the device your IP driver supports, it may be desirable that is reacts correctly in the event that a network connection fails. The driver should not only be able to recognize that the connection is down but also be able to handle the commands that are sent during the period of time when the network is not connected. 

One strategy to detect network connectivity is to enable your driver to send a “heartbeat” is across the network. This could returns a very small amount of data to detect the status of the network.  When several of these returning data packages do not arrive, a disconnect/reconnect is forced. 

If a command is sent while the receiving device is unavailable – the command will fail.  If a command is sent during this timeframe, the command may need to be sent again. While it is possible for your driver to monitor and indicate the status of the network using the OnConnectionStatusChanged function, the command will need to be manually sent yet again if your driver is not written to handle this scenario.

The example in this section is taken from an IP receiver driver. It uses several functions to monitor network connectivity and handle commands accordingly.  It is recommended that OnTimerExpired check to see if the network is connected before commands are sent. In the example below, the driver queries the network three times before it disconnects and then reconnects.


```lua
--global init
gPollTimer = 0

function OnTimerExpired(idTimer)

  if (idTimer == gPollTimer) then
    -- Send a status query on the main port...
    -- If we haven't gotten a response, increment the 'not checked in'... if it hits 3, disconnect all ports...
    gLastCheckin = gLastCheckin or 0
    gLastCheckin = gLastCheckin + 1
    if (gLastCheckin > 2) then
      if (mNetConnected == true) then 
        C4:NetDisconnect(6001, mPort) 
        dbgStatus("Failed to receive poll responses... Disconnecting...")
        C4:ErrorLog("Denon AVR device is not responding... Disconnecting...")
      end
    end
    -- Send Poll Packet if we're ONLINE...
    if (mNetConnected == true) then
      emit('RM ?')
    end
    return
  end

  if (idTimer == gReconnectTimer) then
    dbgStatus("Attempting to reconnect to Denon AVR...")
    C4:ErrorLog("Attempting to reconnect to Denon AVR...")
    local bTrying = false
    gReconnectTimer = C4:KillTimer(gReconnectTimer)
    if (gNetworkStatus == "OFFLINE") then
      -- Try to reconnect to the Denon Control port...
      C4:NetDisconnect(6001, mPort)
      C4:NetConnect(6001, mPort)
      bTrying = true
    end
    if (bTrying) then
      gReconnectTimer = C4:AddTimer(5, "SECONDS")
    end
  end

end

function OnConnectionStatusChanged(idBinding, nPort, strStatus)
  dbg("OnConnectionStatusChanged[" .. idBinding .. " (" .. nPort .. ")]: " .. strStatus)
  if (idBinding == 6001) then
    if (strStatus == "ONLINE") then
      mNetConnected = true
      C4:ErrorLog("Connected to Denon AVR...")
      if (gPollTimer = 0) then C4:KillTimer(gPollTimer) end
      gPollTimer = C4:AddTimer(5, "SECONDS", true) -- 5 second repeating timer...
    else
      mNetConnected = false
      -- Try a re-connect of the device gGCControlPort...
      if (gReconnectTimer == nil) or (gReconnectTimer == 0) then
        gReconnectTimer = C4:AddTimer(5, "SECONDS")
        C4:NetDisconnect(6001, mPort)
        C4:NetConnect(6001, mPort)
      end
    end
  end
end


function processSerial(retMessage)
  gLastCheckin = 0
end

function OnDriverDestroyed()
  -- Kill all timers in the system...
  if (gReconnectTimer = nil) then gReconnectTimer = C4:KillTimer(gReconnectTimer) end
  if (gPollTimer = nil) then gPollTimer = C4:KillTimer(gPollTimer) end
end
```
