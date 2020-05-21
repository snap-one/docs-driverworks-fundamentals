## Replacing a .c4i file with .c4z

An XML element has been included to support the replacement of an existing driver.c4i file with a new driver.c4z. An example of the replaces element is to the right:

```xml
<replaces>
   <replace>driver.c4i</replace>
</replaces>
```

This element is included in the .c4z driver.xml file. It is optional and if used, requires at least one instance of replace containing the .c4i file name. Once the .c4z is loaded, Director will remove the .c4i file from the project. The .c4i file is then ed as "Obsolete" upon driver searches. Obsolete drivers cannot be loaded into a project.

