## ACTIONS

Use the `<actions>` section to define programming which is specific to initial installation/configuration of the driver. This is typically functionality which is not required for ongoing driver use.

Actions are defined in the driver's XML and consist of the following types: STRING, LIST, RANGED INTEGER, RANGED FLOAT and COLOR SELECTOR.

Included is an example of XML containing Actions with Parameters.

	       <action>
	                <name>Test Action with Parameters</name>
	                <command>TestActionWithParameters</command>
	                <params>
	                    <param>
	                        <name>Test STRING password</name>
	                        <type>STRING</type>
	                        <password>true</password>
	                    </param>
	                    <param>
	                        <name>Test STRING</name>
	                        <type>STRING</type>
	                    </param>
	                    <param>
	                        <name>Test LIST</name>
	                        <type>LIST</type>
	                        <items>
	                            <item>On</item>
	                            <item>Off</item>
	                        </items>
	                    </param>
	                    <param>
	                        <name>Test RANGED_INTEGER</name>
	                        <type>RANGED_INTEGER</type>
	                        <minimum>79</minimum>
	                        <maximum>120</maximum>
	                    </param>
	                    <param>
	                        <name>Test RANGED_FLOAT</name>
	                        <type>RANGED_FLOAT</type>
	                        <minimum>50.5</minimum>
	                        <maximum>60.3</maximum>
	                    </param>
	                    <param>
	                        <name>Test COLOR_SELECTOR</name>
	                        <type>COLOR_SELECTOR</type>
	                    </param>
	                </params>
	       </action>

When clicking on the Actions tab from within the Properties window in ComposerPro, the option to Test Action with Parameters is presented:

todo img 1


Clicking this button displays all of the driver's Actions along their parameters. Note that only the default parameter is displayed in this window.

todo img 2

