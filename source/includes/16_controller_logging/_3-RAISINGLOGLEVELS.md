## Raising Log Levels

**Using C4 ToolBox**

C4 ToolBox raises the controller's logs, including `director.log` and `driver_log.log`, to DEBUG:

1. In Composer Pro, select **Tools** > **C4 ToolBox**. Under **Support Functions**, select **Logging**.
2. In **Logging Options**, select **Enable Debug Logs**.
3. In **Choose a Control4 Device**, select **Search Network**. If the controller isn't found, select **Add IP** and enter its IP address.
4. Select the primary controller (listed in green) and select **Enable Debug Logs**.

When testing is complete, select **Reset Logs to Default Level** in **Logging Options** and choose the same controller.

**Using System Diagnostics**

To set the level of a single logger, in Composer Pro, select **Tools** > **System Diagnostics** and select the **Logging** tab. Set **Logger** to the logger you want to change, such as `director`, and set **Log Level** to the level you want.

`driver_log.log` is not one of the loggers listed in System Diagnostics. To raise it, use C4 ToolBox.

Because `director.log` fills and rotates quickly, use a controller snapshot rather than **Start Tail** to review driver messages.
