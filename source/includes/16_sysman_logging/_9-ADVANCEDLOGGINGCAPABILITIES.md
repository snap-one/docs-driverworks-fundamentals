## Advanced Logging Capabilities


**Determining the Status of a Log File**
Using the sysman logging interface you can ascertain the status of a log file using the first example to the right:

```js
/# sysman log director
lighttpd syslog err enabled file /var/log/director.log
OK
```

In this example, we can see that the director logging utility is set to a level of ERROR and it is using a file appender with a directory destination of: /var/log.



**Assigning Log Levels**
Prior to 2.6.0, the only way to change a logging utility's level was to edit its .config file and then restart the controller. 

The Sysman Logging utility allows you to assign log levels to a specific log through its interface.  For example, if we wanted to set the logging level for our director log to DEBUG, we would enter the code from the second example:  

```js
/# sysman log director debug
 Logging for director set to debug
 OK
```

In the third example, we can also return the log to value its default value of ERROR.

```js
/# sysman log director default
  Logging for director set to default
  OK
```

Note that Logs for programs are always enabled. It is not possible to disable logging for a program. You can only reset it to its default value. Supported Log Levels include:

| Log Level | Description |
| --- | --- |
| Fatal | The FATAL level designates very severe error events that will presumably lead the application to abort. |
| Error | The ERROR level designates error events that might still allow the application to continue running. |
| Warning | WARN level designates potentially harmful situations. |
| Info | INFO level designates informational messages that highlight the progress of the application at coarse-grained level. |
| Debug | The DEBUG Level designates fine-grained informational events that are most useful to debug an application. |
| Trace | The TRACE Level designates finer-grained informational events than the DEBUG. |

By default, logging is enabled for all Control4 processes. The default log level is Error, meaning Fatal and Error level messages are logged. Warning, Info, Debug, and Trace messages are not.



**Targeting Log Directories**

Prior to 2.6.0, the only way to change a logging utility’s destination directory was to edit its .config file and then restart the controller. 

Beginning with OS 2.6.0, the use “Log Appenders” is supported. Log Appenders deliver a log file to a specific destination. The assignment of a Log Appender cannot be done through the sysman interface. It must be done through an SSL connection. There are three kinds of Log Appenders supported by the sysman logging interface:

- syslog appenders - These send log messages to syslog.
- file appenders - These write log messages to a file. 
- socket appenders - These send log messages to a specific host and port number.

Most Control4 programs use a simple file appender at the root logger. The sysman log interface allows users to add additional appenders, as well as remove appenders. The following example shows these actions using the director log:

First, we’ll set the log level using the same syntax used previously:

```js
/# sysman log director debug
Logging for director set to debug
OK
```

Next, we’ll add a new destination directory for the director’s log file. To do this we need to add a file appender:

```js
/# sysman log director add /tmp/my-director.log
Added file appender director_0 to director for /tmp/my-director.log
OK
```

The code sample above adds a new file appender which will write the log file to: /tmp/my-director.log. 

Note that the confirmation message displayed by sysman also includes the appender name or `director_0` in the example above.

The `director_0` name is also used when we need to delete the appender:

```js
/# sysman log director remove director_0
Removed appender director_0 from director
OK
```

Any log file destination must begin with a slash "/" character as this creates the log4cplus file appender.

If the destination is a host:port number, for example "192.168.0.100:5400", sysman will create a socket appender and configure it for the given host and port.

If the destination is a host name, for example, "director" (this is the host name of the controller running the director process), sysman will create a syslog appender and send log messages there. Note that the target syslog process must be configured to receive these messages and route them appropriately.

For Example:

```js
/# sysman log director add /tmp/tmp_director.log
Log file for director set to /tmp/tmp_light.log
OK
```



**Log File Size and Rotation Settings**
Configuration of log file size and rotation setting has not changed with new sysman logging utility. These can be configured in the logrotate.conf file on the controller. The file can be edited using a text editor.



**Reviewing the Status of All Active Loggers**
An overview of the status of all loggers can be displayed through the sysman command line by entering: sysman.log
The resulting output is to the right:

```js
 Logger            Type      Level    Status      Appender         Destination 
 audio3client      program   error    defaulted   rootAppender     /var/log/audio.log
 audio3client_nl   named     error    defaulted
 audio3server      program   error    defaulted   rootAppender     /var/log/audio.log
 audio3server_nl   named     error    defaulted
 audio_asp_nl      named     error    defaulted
 audio_buffer_nl   named     error    defaulted
 audio_utils_nl    named     error    defaulted
 audioclient       program   error    defaulted   rootAppender     /var/log/audio.log
 audioclient_nl    named     error    defaulted
 audioserver       program   error    defaulted   rootAppender     /var/log/audio.log
 audioserver_nl    named     error    defaulted
 c4lookup          program   error    defaulted   rootAppender     /var/log/c4lookup.log
 c4lookup_nl       named     error    defaulted
 c4net_nl          named     error    defaulted
 c4perfd           program   error    defaulted   rootAppender     /var/log/c4perfd.log
 c4perfd_nl        named     error    defaulted
 c4rmengined       program   error    defaulted   rootAppender     /var/log/c4rmengined.log
 c4rmengined_nl    named     error    defaulted
 c4server          program   error    defaulted   rootAppender     /var/log/c4server.log
 c4server_nl       named     error    defaulted
 cli_event         named     error    defaulted   cli_event        unknown
 daemon            syslog    debug    set         syslog           director
 default           program   error    set         rootAppender    
 director          program   error    set         rootAppender     /var/log/director.log
```


- The first column titled “Logger” shows the program or named logger.

- The “Type” column shows the type of logger. There are three types of loggers: syslog loggers, program log4cplus root loggers and named log4cplus loggers. Note that the sysman interface doesn’t expose the differences between these types of loggers. However, it is exposed here as Log Type as it may be useful when reviewing groups of loggers.

- The “Level” column, Level, indicates the current logging level.

- The “Status” column Status indicates if the level is set explicitly or derived from the logging defaults.

- The “Appender” column indicates the type of destination. This will be a file, syslog host, named appender or root appender.

- The Destination column shows where log lines are sent. This will be either a file name, host name, syslog ident, or console.