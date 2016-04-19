# Blue Foot: Page Builder
## Config
The page builder relies on a lot of data from Magento. It retrieves this via an Ajax request when the page builder is initialized. The request is defered until the user requests the page builder interface to ensure that we don't interupt their other interaction with the page.

You may need to pass through server side information to your widget or plugin. Commonly this would be extra data, ajax URLs or configuration values.

#### Observer event
You can utilise the Magento `gene_bluefoot_stage_build_config` observer event to modify the build config for the stage. This is the configuration that is passed back to the system on initialize.

You can see the following in the core `config.xml`
```
<gene_bluefoot_stage_build_config>
    <observers>
        <gene_bluefoot_widget_upload>
            <type>singleton</type>
            <class>gene_bluefoot/stage_widget_observer</class>
            <method>stageBuildConfig</method>
        </gene_bluefoot_widget_upload>
    </observers>
</gene_bluefoot_stage_build_config>
```

On closer inspection of `gene_bluefoot/stage_widget_observer` we can see that the `Varien_Event_Observer` contains the configuration under the key `config`.

To add new data into the config you'll need to retrieve the data, modify it and reset it back into the config `Varien_Object.`

For instance:
```
public function stageBuildConfig(Varien_Event_Observer $observer)
{
    /* @var $config Varien_Object */
    $config = $observer->getEvent()->getConfig();
    $pluginData = $config->getData('plugins');

    // Send the upload path URL
    $pluginData['gene_testing']['config']['upload_url'] = Mage::helper('adminhtml')->getUrl('adminhtml/gene_testing/upload');

    $config->setData('plugins', $pluginData);

    return $this;
}
```

This will then add the above keys into the plugins section under the `gene_testing` plugin.

#### Accessing extra configuration data
When the system loads this config it stores it within the page builder's `config.js` file. There are various helper methods within this resource to help you obtain data.

##### Plugin data
If you declare your extra configuration data within a plugin as above you can access it via the 
```
Config.getPluginConfig('gene_testing', 'upload_url');
```

Replacing the first parameter with the plugin name, and the second with the key within config.

##### Other data
You're able to access any other data directly via the `getValue` function. We highly reccomend keeping your extra data within the plugin scope, so to not clutter the configuration.
```
Config.getValue('key');
```


