
## Intercom Proxy Best Practices

### Overview

The Intercom Proxy interface is defined in terms of Commands, Requests and Notifications, each of which are described in this section of the proxy and Protocol Guide.

[Intercom Proxy Commands][1] are issued by a proxy consumer and may or may not result in an asynchronous notification in response.  Commands typically change state of the Intercom Proxy, or other components of the intercom architecture.

[Intercom Proxy Requests][2] are issued from a proxy consumer with the expectation of getting a response from the protocol.  Requests do not change state of the intercom proxy or any other component of the architecture.  

[Intercom Proxy Notifications][3] are sent from the intercom proxy to the intercom consumer asynchronously, and indicate a state change in the intercom proxy or one of its related architecture components.

All communication through the proxy interface is asynchronous.  Even request responses, such as getting a list of known intercom devices, are returned asynchronously with respect to the request for that data.


### Storing Session IDs

Many intercom API commands and requests require the sessionId as an input parameter. The value for this parameter is provided by the SIP Client on the device in response to issuing a [`START_CALL`][4] command or a physical button press event. 

This sessionId value is included in the resulting [`OUTGOING_CALL`][5] or [`INCOMING_CALL`][6] notifications. The intercom proxy consumer is responsible for storing the sessionId returned with these notifications for use in subsequent commands and requests.


### Using the RemoteAor Property

When the “remoteAor” parameter is used in a notification or command, the value is always the address of record of the other intercom device participating in an intercom call. 

For example, consider that intercom device A has address of record of “sip-user-A@192.168.1.123” and intercom device B has an address of record of “sip-user-B@192.168.1.123”. 

Intercom device A initiates a call with intercom device B, which results in an [`INCOMING_CALL`][7] notification on intercom device B. When intercom device B issues the [`ACCEPT_CALL`][8] command, which takes remoteAor as a parameter, intercom device B should use “sip-user-A@192.168.1.123” (the address of record of intercom device A, or the initiator) as the value of this parameter. 

Likewise, when intercom device A sends the [`CALL_ACCEPTED`][9] notification, it will contain a remoteAor property that has the value of the address of record of intercom device B, or “sip-user-B@192.168.1.123”.


For additional Information, please see the [DriverWorks Proxy and Protocol Guide][10] regarding all of the functions supported by the Intercom Proxy.


[1]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-commands
[2]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-requests
[3]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-notifications
[4]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-commands-start-_call
[5]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-notifications-outgoing_call
[6]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-notifications-incoming_call
[7]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-notifications-incoming_call
[8]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-commands-accept_call
[9]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#intercom-call-notifications-call_accepted
[10]:	https://literate-tribble-gq5lk61.pages.github.io/#introduction