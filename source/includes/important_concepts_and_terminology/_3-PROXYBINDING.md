## Proxy Binding


The proxy driver and the protocol driver are connected together with what are called proxy bindings.  The Control4 system sends general command such as PLAY, STOP, PAUSE, etc. to the proxy driver which is bound to the protocol driver.  The protocol driver sends specific commands that the device understands to the device over its connected communication mechanism, IP, serial, etc.