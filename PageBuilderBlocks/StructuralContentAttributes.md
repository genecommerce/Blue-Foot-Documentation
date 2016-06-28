# Blue Foot: Page Builder Blocks
## Structural Content Attributes
Content attributes for structural elements such as rows and columns cannot be added using the admin interface, these are defined in XML.  You will see these defined within a structural handle in app/code/community/Gene/BlueFoot/etc/bluefoot/pagebuilder.xml.

##### Adding a Structural Content Attribute
To add a structural content attribute you must first create a pagebuilder.xml within your module.

app/code/community/[Namespace]/[Module]/etc/bluefoot/pagebuilder.xml

From here you can add your own attribute.
```
<structural>
    <ELEMENT>
        <fields>
            <template>
                <label><![CDATA[]]></label>
                <type></type>
                <code></code>
                <widget></widget>
                <source_model></source_model>
            </template>
        </fields>
    </ELEMENT>
</structural>
```

ELEMENT should be either row or column.

To use a widget rather than a standard input pass 'widget' into the type argument and then define the widget as a separate argument.

##### Accessing your Attribute in a phtml file
To access your new attribute within your phtml files you can simply pass the attribute code to the 'getFormData' function.

```
<?php echo $this->getFormData(ATTRIBUTE_CODE); ?>
```
