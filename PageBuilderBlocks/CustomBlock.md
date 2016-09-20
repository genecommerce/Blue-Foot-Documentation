# Blue Foot: Page Builder Blocks
## Creating a custom block
Creating a custom block wth BlueFoot is easy. The first thing you will need to do is create and register your custom blocks and templates.

#### Adding an custom page builder xml file

To register custom templates and blocks you must first create a pagebuilder.xml within your module.
app/code/community/[Namespace]/[Module]/etc/bluefoot/pagebuilder.xml

```
<?xml version="1.0"?>
<config>
</config>
```

#### Registering a template

To register a template add the following code to your custom pagebuilder.xml file.
```
<content_blocks>
    <templates>
        <!-- Custom -->
        <TEMPLATE_IDENTIFIER>
            <file><![CDATA[gene/bluefoot/pagebuilder/blocks/custom/your-template.phtml]]></file>
            <assets>
                <CSS_IDENTIFIER>
                    <type>js_css</type>
                    <name>gene/bluefoot/resource/your.css</name>
                </CSS_IDENTIFIER>
                <JS_IDENTIFIER>
                    <type>js</type>
                    <name>gene/bluefoot/resource/your.js</name>
                </JS_IDENTIFIER>
            </assets>
        </TEMPLATE_IDENTIFIER>
    </templates>
</content_blocks>
```

TEMPLATE_IDENTIFIER should be a unique identifier for your template. e.g. awesome_custom_block
To load assets with your template please identify them inside 'assets'.
CSS_IDENTIFIER & JS_IDENTIFIER are simple unique identifiers for the assets.
If you want a custom admin template BlueFoot will use the same template within your admin theme.


#### Registering a block

If your custom block needs it's own set of functions it's recommended to create a custom block for it. Simple blocks should be fine using the blocks that already exist within BlueFoot.
Registering a block works in exactly the same way as registering a template. First open your pagebuilder.xml file and add the following code.

```
<content_blocks>
    <renderers>
        <BLOCK_IDENTIFIER>
            <class><![CDATA[Namespace_Module/Block_Name]]></class>
        </BLOCK_IDENTIFIER>
    </renderers>
</content_blocks>
```

#### Creating a block

Now you have registered a template and block it's time to get started creating your block.
This can all be done within the admin. You can see visuals of these pages on the [Overview](Overview.md) page.

**Step 1** - Create your attributes
If you want your block to have attributes that don't already exist you will need to create them. Doing this is simple, first visit System->BlueFoot->Content Attributes.
From here you can create attributes in the same way you would product attributes.

**Step 2** - Create your block
Go to System->BlueFoot->Page Builder Blocks.  From here you can creates blocks easily. Remember to hit 'Use in Page Builder' and to assign your template and box.

#### Creating your template

The last step is to create your templates. Once your have created the file specified in your pagebuilder.xml you can add what ever markup you like :-)
An example below shows how to load the title attribute in a template.

```
<?php $_entity = $this->getEntity(); ?>

<h1><?php echo $_entity->getTitle(); ?></h1>
```

#### Creating child blocks

Coming Soon
