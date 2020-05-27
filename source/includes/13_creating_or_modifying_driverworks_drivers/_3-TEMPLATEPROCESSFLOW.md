## Template Development Process Flow

This section provides step by step directions to help you create the .c4i file needed for your DriverWorks driver.  In most cases you will want to either use a template or use an existing driver .c4i file and modify it to meet your needs.  While it is possible to create a .c4z file by hand using the information provided above, Control4 does not recommend that approach due to the complexity and many required elements.

As you can see by the following steps, there is a great deal of similarity between using a template or sample DriverWorks driver and using an existing 1-way driver’s .c4z file.  The differences specifically lie in which fields need to be changed.

Several templates have been included in the SDK.  These templates and sample drivers range from very simple .c4z’s to partially functional DriverWorks drivers.  If a template exists for the device (proxy) type that you are creating your driver for, it is very easy to begin with the pre-existing template.

The steps are as follows:

1. Copy the template .c4z and rename your copy
2. Change the .c4z elements that you need to change in order to make your driver.
3. Fields that should be changed include: `<manufacturer>, <model>, <creator>, <name>, <created>, <modified>`
4. Fields that may need modification include: `<documentation>, <properties>, <actions>, <commands>, <connections>`
5. After each major change you make ensure that your .c4z file can be seen by and loaded into your project using Composer. It is good practice to verify this throughout the time you work on the .c4i file.
6. Add/change the Lua code that will be used by your driver.
7. After all the change iterations have been completed, reload your driver 


Occasionally, and particularly for AV devices, you might find it easier to take an existing one-way driver’s .c4z file and convert it for use as a DriverWorks driver.  This enables you to leverage all the connections and capabilities information required to model your device.

The steps are as follows:

1. Copy the existing one-way driver’s .c4z file and rename your copy
2. Change the .c4z elements that you need to change in order to make your driver.
3. Fields that must be changed include: `<driver>, <control>`
4. Fields that should be changed include: `<creator>, <name>, <created>, <modified>`
5. Fields that may need modifiction include: `<manufacturer>, <model>, <documentation>, <properties>, <actions>, <commands>, <connections>`
6. After each major change you make ensure that your .c4z file can be seen by and loaded into your project using Composer. It is good practice to verify this throughout the time you work on the .c4i file.
7. Add/change the Lua code that will be used by your driver
8. After all the change iterations have been completed, reload your driver.