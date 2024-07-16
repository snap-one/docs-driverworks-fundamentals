
## Amplifier Proxy and Navigator EQ Controls

The ability to display audio equalization controls on devices running Navigator was added in conjunction with Operating System 3.2.3. Currently, the only Proxies that support Navigator EQ controls are the Amplifier and AV Switch Proxies.

The majority of the of the commands, notifications and capabilities defined here must be implemented in your driver to display EQ controls. Several commands are optional including SET\_AUDIOMODE\_BYPASS, SET\_AUDIOMODE\_EQ and SET\_AUDIOMODE\_TONECONTROL as these have been included to suspect legacy Audio Matrix Switches.

Note that this functionality is disabled by default. The HIDE EQ FROM NAVS CHANGED notification must be sent with a value of False to enable the EQ functionality in Navigator.

For additional Information, please see:

[Navigator EQ Amplifier Proxy Commands][1]

[Navigator EQ Amplifier Protocol Notifications ][2]

[Navigator EQ Amplifier Capabilities][3]

[1]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#amplifier-navigator-eq-navigator-eq-proxy-commands
[2]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#amplifier-navigator-eq-navigator-eq-protocol-notifications
[3]:	https://snap-one.github.io/docs-driverworks-proxyprotocol/#amplifier-navigator-eq-navigator-eq-capabilities