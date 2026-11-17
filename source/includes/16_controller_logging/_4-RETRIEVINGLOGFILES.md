## Retrieving Log Files

To retrieve the log files, download a controller snapshot: in Composer Pro, select **Tools** > **Device Logs**, then select **Download** for the primary controller. The download can take a few minutes.

The snapshot is a .zip file that mirrors the controller's log directories and includes the rotated, compressed copies of each log (for example, `director.log.1.gz`). The log files of most interest to driver developers are:

| Log File | Contents |
| --- | --- |
| `var/log/debug/director.log` | Director's log. Includes messages drivers write with `C4:DebugLog` and `C4:ErrorLog`. |
| `var/log/debug/driver_log.log` | Messages drivers write with `C4:DebugLog` and `C4:ErrorLog`. |
| `var/log/log/driver_debug.log` | Messages Director reports about drivers, such as load failures and runtime errors. |

Log files in `/var/log/debug` are stored in RAM and are cleared when the controller reboots, so download the snapshot before rebooting. `driver_debug.log` is not cleared on reboot.

For more information, see the DebugLog and ErrorLog sections of the DriverWorks API Reference Guide.
