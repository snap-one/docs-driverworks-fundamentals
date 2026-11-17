## Log Levels

Each logger writes only the messages at its current level or a more severe level. The supported log levels, from most to least severe, are:

| Log Level | Description |
| --- | --- |
| Fatal | The FATAL level designates very severe error events that will presumably lead the application to abort. |
| Error | The ERROR level designates error events that might still allow the application to continue running. |
| Warning | The WARN level designates potentially harmful situations. |
| Info | The INFO level designates informational messages that highlight the progress of the application at a coarse-grained level. |
| Debug | The DEBUG level designates fine-grained informational events that are most useful to debug an application. |
| Trace | The TRACE level designates finer-grained informational events than DEBUG. Use it sparingly, during the active investigation of an issue. |

By default, release builds of Control4 OS log at ERROR, so only Fatal and Error messages are written. Messages a driver writes with `C4:DebugLog` do not appear until the log level is raised to DEBUG or TRACE.

Raise log levels only while testing, and return them to their default levels when testing is complete. Higher log levels write more data, and logs such as `director.log` then fill and rotate more quickly.
