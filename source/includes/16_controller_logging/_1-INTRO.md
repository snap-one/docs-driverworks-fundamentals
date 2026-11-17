## Introduction

Control4 OS writes log files on the controller for Director and the other Control4 processes. Driver developers use these logs to see messages their drivers write with `C4:DebugLog` and `C4:ErrorLog`, and messages Director reports about drivers, such as load failures and runtime errors.

Shell access to the controller (SSH or PuTTY sessions) is no longer available, and the System Manager (sysman) utility and its command-line logging interface are not supported for driver development. Earlier versions of this guide described changing log levels, adding log appenders, and viewing log files with sysman. Use the Composer Pro tools described in this section instead:

- **C4 ToolBox** raises the controller's logs to DEBUG and resets them to their default levels.
- **System Diagnostics** sets the log level of an individual logger, such as `director`.
- **Device Logs** downloads a controller snapshot that contains the log files.

Log destinations and log rotation cannot be changed.
