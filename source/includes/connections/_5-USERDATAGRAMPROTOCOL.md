## User Datagram Protocol

Note that there's no need for the use of `C4:NetConnect` and `C4:NetDisconnect` when using UDP as every packet is a UDP datagram and is independent of any connection. Also, the usage of network functions `C4:SendToNetwork`  and `C4:ReceivedFromNetwork` are the same as if using TCP.


	<connection>
		<id>6001</id>
		<facing>6</facing>
		<connectionname>ADAM Network Connection</connectionname>
		<type>4</type>
		<consumer>True</consumer>
		<audiosource>False</audiosource>
		<videosource>False</videosource>
		<linelevel>True</linelevel>
		<classes>
			<class>
				<classname>UDP</classname>
				<ports>
					<port>
						<number>1025</number>
	                    <auto_connect>True</auto_connect>
	                    <monitor_connection>True</monitor_connection>
	                    <keep_connection>True</keep_connection>
	                </port>
	            </ports>
	        </class>
	     </classes>
	</connection>

