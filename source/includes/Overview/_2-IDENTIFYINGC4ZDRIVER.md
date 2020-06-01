## Identifying a .c4z Driver

The ability to specify a unique driver identification image and instructional text can be defined in the `\<config\>` section of your .c4z driver. 

```xml
<devicedata>
    ...
    <config>
        <identify_text>Insert how to identify here</identify_text>
        <identify_image>my_identify.gif</identify_image>
    </config>
    ...
</devicedata>
```
 

Both the text string and the .gif are pulled from the .c4z file at the relative path specified in the driver. In the example above they would just be in the root of the c4z.
 
For identify images, .gifs are recommended as they can be animated. Other formats should work fine, however.
  