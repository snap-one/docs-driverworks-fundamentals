## Persisting Driver Data and the PersistData Table

The PersistData table was created with the initial release of the DriverWorks SDK. It still works in the current OS releases and it is anticipated that it will continue to work in subsequent releases. Prior to OS 2.10.0 InvalidateState caused the PersistData table to be stored. However, InvalidateState was overused and caused significant performance issues on larger projects. 

Beginning with OS 2.10.0, InvalidateState no longer functions. Now, the PersistData table is systematically saved out. This is done by a Director Timer that fires twice per second. Each time the timer fires, Director round-robins through the drivers in the project and saves off any state information for that specific driver. Due to this model, the larger the project - the longer it will take to persist the data.

Several factors should be considered when deciding if data should be persisted. If the data is significant enough that it should be persisted in the project file (.c4p); then it likely is a good candidate for being stored in the PersistData table or being handled through the creation of a Property. One example to consider is a project reload. When a project is loaded, the data residing in the PersistData table should be populated with data that will be used to bring the system back to the stable, usable state. An example of this type of data would be the ramp rate levels configured for lights or LED colors configured on keypads. Those are data values that should be available as soon as the project is loaded. 

Additionally, PersistData should be properly retained when any of the following occur:

- The Composer user saves (backs up) the project 
- Director restarts in an expected manner 
- The driver is updated using Composer or Auto-update methods

Other types of runtime data that offer no benefit to being persisted should not be stored in the PersistData Table. The last known power state of a light (On/Off) or the last media instance being streamed are examples of data that should not be stored in the project file.
Note that functions cannot be members of a table that contain data that is intended to be persisted.

### Persisting Data Post O.S. 2.10.0 
Drivers that were created for OS 2.10.0 and later will benefit from using the persist functions delivered in the 2.10.0 release. They include:

- PersistSetValue 
- PersistGetValue 
- PersistDeleteAll

These APIs are defined in the DriverWorks SDK API Reference Guide. They do not depend on the same round-robin handling for saving data that is described above. These are handled by a Director thread and are very quick.

It is important to understand that persisting extraneous data through a driver can have performance implications. Device drivers should avoid calling these functions spuriously as this will result in the data never being saved. Director will only flush data to the disk after a sufficient period of no changes by any driver. Currently, a save of all persist data for all drivers is very CPU intensive. To view persisted data, see the PersistData Table in the driver XML and the Properties interface in ComposerPro for values that are being persisted through your driver.
