## .c4z File

Beginning with operating system 2.6.0, Control4 changed the file structure previously used to support device drivers. Up until 2.6.0, device driver were contained within a single-level file using the extension of .c4i or: `drivername.c4i`

The release of 2.6.0 marked a significant change to this model with the introduction of the .c4z file structure or: `drivername.c4z`. A .c4z file provides all of the content that the .c4i file previously did. However, it offers some significant features and advantages that its predecessor did not.

At its most basic level, a .c4z file is a zip file. It contains numerous folders and files which, when encapsulated in a .c4z file, represent a device driver. The modularity within the .c4z file makes for a far more organized and structured approach to device driver architecture. It also provides the ability to include many related objects within the confines of the driver in an organized manner.

See Understanding .c4z Driver Files for information regarding Control4's latest driver file architecture.