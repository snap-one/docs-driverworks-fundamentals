
Beginning with O.S. Release 3.1.3, the DriverWorks SDK provides a stable enough platform for developing Control4 Agents in addition to device drivers. DriverWorks Agents provide third-party Driver Developers the ability to create agents which can be loaded into projects and used in customer homes.

### Creating an Agent from an Existing Driver

1. Select the DriverWorks driver. Preferably a self-proxy or combo driver.
2. Ensure that your favorite driver is actually a .c4z type driver.
3. Open your driver and select the “driver.xml” for editing.
4. Add the following child xml element to the `<devicedata>` element:  `<agent>true</agent>` For example:

	 `<devicedata>`
	   ` <agent>true</agent>`
	 `<devicedata>`

5. Save the driver.xml back into your favorite selected driver.
6. Load the updated driver into a Control4 project.


### Managing DriverWorks Agents

Also beginning with O.S. Release 3.1.3, Composer creates a new “Agents” directory in the “My Documents\Control4” directory structure. It is recommended that DriverWorks Agents are placed into this directory. Maintaining new Agents in this directory makes them easy to locate and avoids possible confusion for dealers who may try to load the agent as a driver. DriverWorks Drivers which have been modified to an agent will not load properly if treated as a driver.


### Loading a DriverWorks Agent

To load a new DriverWorks Agent, select Agents from below the Project Tree in Composer Pro. From within the Agents panel, click on Add. The new Agents screen is displayed. 

<img src="images/22_1-01.png"/>


This screen has a very similar look and function as that of the Drivers Search tab under System Design. All of the project’s legacy agents as well as newly created DriverWorks Agents are displayed in the results window. The search window at the top of the screen allows for text-based searches. Furthermore, searching for Agents can be refined by Category and Manufacturer. Search results can be sorted by the Control Method defined in the Agent as well the relevance which includes sorting by name or modified date. 

Note that DriverWorks Agents will be available for distribution through the Online Driver Database in a future release..

