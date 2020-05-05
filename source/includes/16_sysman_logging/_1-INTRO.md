## Introduction

Beginning with OS system 2.6.0, the System Manager (sysman) utility has been enhanced with a logging interface. This interface has been delivered as a replacement for the previously supported c4log utility. The sysman log interface represents an enhancement to the previous supported system logging process. Some improvements include:

- Deprecation of the c4log script. This has been replaced with enhanced logging control commands executed from within System Manager.

- Unified the existing logging levels and their messaging to provide consistency across logging output.

- Increased the number of logging utilities to deliver a more granular view of log content.

- Increased the number of logging utilities including independent, targeted logs and increased log level control to reduce the amount of times Director logs roll over.

- Added advanced logging features such as named loggers and appenders. This provides the ability to direct logs to other directory and network locations. This will prevent the loss of information which previously occurred due to system re-boots.

- Added a new log level: TRACE. This level logs an even greater level of detail than DEBUG. This should be used sparingly during the active investigation of an issue.

- Default functionality now sets Log levels to INFO rather than DEBUG unless tracking down an issue. This is only applicable in  versions. Release version logging defaults to DEBUG.

- Default functionality now sets logging to be enabled in a release version capturing FATAL and ERROR conditions.

- The ComposerPro System Manager UI has been enhanced to support the logging interface. Logs can be viewed and filtered and log levels assigned through System Manager.

The purpose of this section is to provide instruction on how to use the sysman logging platform to:

- Connect to a controller from the System Manager Logging UI

- Review active Loggers within the UI

- Filter the results displayed in the UI

- Assign a logging level to an active Logger from within the UI

- View Log File content and Log File locations

The Advanced Logging Capabilities section provides command line instructions on how to accomplish all of the above as well as information on how to assign Log Appenders.
