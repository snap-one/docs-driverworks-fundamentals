# Markdown Driver Documentation
Documentation can play an important role in ensuring the proper configuration and use of a driver. Driver documentation is created by the driver writer and delivered within the driver file. After the driver is added to a project, the content is displayed in ComposerPro under the driver’s Advanced Properties/Documentation tab.

Several approaches to delivering driver documentation are currently supported. They differ mainly due to the documentation needs required by the driver.

**driver.c4i**

If the driver you’re writing uses the .c4i extension, the documentation content is delivered in the XML section of the driver. The content is included within the `<documentation></documentation>`tags. This is the only option to include driver documentation when using the .c4i extension.


**driver.c4z**

If the driver you’re writing uses the .c4z extension, the documentation content can be delivered in two ways. The first involves the inclusion of a documentation.rtf file within the www directory at the root level of driver. While .RTF files are somewhat limited in nature they can be appropriate for supporting drivers that don’t require the need for a robust documentation experience. Detailed information on the inclusion of a documentation.rtf file can found here:

[https://snap-one.github.io/docs-driverworks-fundamentals/#understanding-c4z-drivers][1]

The second way to include driver documentation within a .c4z driver file is to first created the content in Markdown and then render the Markdown into HTML using React. This approach offers the simplicity of creating content with Markdown while benefitting from the features provided through HTML.

The DriverWorks SDK includes all of the React assets needed to render Markdown driver documentation into HTML. The assets are included in the release.zip file found in the on the DriverWorks Landing page here:

[https://github.com/snap-one/docs-driverworks][2]

To include Markdown documentation in your driver.c4z file, first locate the markdown\_driver\_doc folder at the top of the DriverWorks landing page. Note that the folder contains a release.zip file.

1. Unzip the release.zip file into the www/documentation folder in your driver .c4z file. The updated documentation directory structure should look like this:


<img src="images/2_01.jpg"/>


Note that the 'x' characters seen above will be replaced with random letters or digits.

2. Save the new driver documentation in Markdown as a file named “documentation.md” at the root of the www directory.
3. Any images used within the documentation need to reside in a subfolder named img at the root of the www directory. An example of a www directory with assets, a driver documentation.md file and associated images would look like this:

<img src="images/2_02.jpg"/>

Note that the documentation.md file **is not** inside the documentation folder.

4. Finally, it will be necessary to point to the index.html file in the documentation XML element of the driver.xml:


	 `<documentation file="www/documentation/index.html"/>`


After the driver.c4z file is compiled, the assets included from the release.zip will generate driver documentation using the same formatting as can be seen in drivers delivered by Control4.

[1]:	https://snap-one.github.io/docs-driverworks-fundamentals/#understanding-c4z-drivers
[2]:	https://github.com/snap-one/docs-driverworks