## What was New in 3.1.3

### Driver Agents

Beginning with O.S. Release 3.1.3, the DriverWorks SDK provides a stable enough platform for developing [Control4 Agents][1] in addition to device drivers. DriverWorks Agents provide third-party Driver Developers the ability to create agents which can be loaded into projects and used in customer homes. _Please see the Driver Agents section for more information._


### Future Change to Zigbee Server-Side Cluster Management in OS 3.2.0

Starting with the upcoming release of Operating System 3.2.0, Control4 will change the way Zigbee Clusters are handled. Drivers that have previously relied on the ability of clusters to circumvent zserver and its processes will be impacted by this change and will not function as intended in OS 3.2.0 and going forward.

For example, consider the following sample Profile:

(HA Profile (0x0104)
Cluster A: Time (0x000A)
Cluster B: Basic (0x0000)
Cluster C: OTA Upgrade (0x0019)

Pre-OS 3.2.0, these clusters and others may have been partially handled by Control4's zserver. With the release of OS 3.2.0, Time, Basic and OTA clusters will be advertised as supported by Control4, via a MatchDescriptorRequest, on the HA Profile (0x0104) and will be entirely handled within zserver.

Note that there are two scenarios where zserver will pass a cluster un-handled directly to a driver. These are:

- If the cluster that has the manufacturer-specific flag set, the cluster will be passed through to the driver, un-handled by zserver.

- All clusters which are not supported by Control4, on the HA profile, will be passed through to the driver, un-handled by zserver.

To avoid unintended functionality, a review of your Zigbee driver(s) is recommended prior to the release of OS 3.2.0. The ability to send Zigbee Clusters directly to your driver will need to be handled at the device firmware level.

Operating System 3.2.0 will include an OTA Upgrade API which will provide a driver based approach to device updates.


### SQLite3 Database Information

Control4 has been delivering a SQLite3 library with its Lua distribution beginning with O.S Release 2.10.0. This library is delivered without the intention of being a third party developer resource. For this reason, Control4 does not provide an interface into the library and has no intention of providing an interface or supporting third party developers in efforts to utilize SQLite3. If the decision is made to use this database Control4 strongly encourages developers to familiarize themselves with the content available through sqlite.org and the LuaSQLite 3 wiki.

Several issues must be considered when using SQLite3. Please ensure that in your development efforts you avoid doing things that fall into the categories below and/or account for the limitations they can present:

- SQLite3 utilizes a blocking API. This means that Director will be blocked while read/writing database files. Reads aren't generally a concern, but writes can be expensive. Especially when the transaction API is not used.

- Access to the filesystem: driver developers should only open database(s) in their sandbox directory. Database files opened and/or placed elsewhere on the filesystem may have unintended consequences causing problems with the Control4 Operating System.

- Database files will not be backed up with a project since the backup mechanism knows nothing about these files. This means data stored in the driver’s DB file will get lost if the controller is ever restored to default or migrated to a new controller.

- The controller has limited disk space. It is important to be aware of the amount of data that driver developers collect/store and ensure that the DB doesn’t grow to a point where it will impact the controller’s disk space. This may mean that driver developers may need to have a mechanism for archiving data to off-controller storage and prune the local DB if they want to maintain historical data to the extent that the DB file size becomes too large.

[1]:	https://control4.github.io/docs-driverworks-fundamentals/#driver-agents