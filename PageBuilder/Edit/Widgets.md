# Blue Foot: Page Builder
## Widgets

Widgets in Blue Foot allow a developer to create a new type of field within the Blue Foot page builder. This can provide extended functionality upon a field, a number of core widgets are provided by default:

- Color
- Date
- WYSIWYG
- Child Blocks
- Align
- Metrics
- Tags
- Upload

Some of these are used to mimic similar fields available in core Magento, for instance the WYSIWYG field creates an instance of TinyMCE within the page builder.

#### XML Configuration
The system allows you to easily, and dynamically create new widgets without having to modify any core components.

A widget has to be created as part of a complete Magento module. This module will contain a *genecms.xml* file. Within this file we are able to declare our widgets. The declaration in this file creates an alias within *RequireJS* to allow the system to automatically load your widget when it's assigned to a field.

Here we can see the structure of the product search widget:
```
<config>
    ...
    <widgets>
        <gene_search_product translate="label">
            <label>Product Search</label>
            <alias>search/product</alias>
            <input_type>text</input_type>
            <group>Blue Foot</group>
            <data_model>gene_cms/attribute_data_widget_product</data_model>
        </gene_search_product>
    </widgets>
    ...
</config>
```

There are 4 XML tags that have to be declared for a widget to be successfully loaded by the system. A widget doesn't need a data model, a data model should only be used if extra data is required.

- **label**: The widgets name as displayed to an administrator
- **alias**: The widget's alias that will be used to load this widget within the page builder
- **input_type**: The default input type this widget will save as, this is important to ensure you're storing your data in the correct EAV table.
- **group**: The group which this widget belongs to, this should be the company name or content app that the widget is included within. Core widgets are displayed in the Blue Foot group.
- **data_model**: If the widget needs to complete any further processing to display data contained within the field it can implement a data model. This is used in a number of core widgets to load product, category and static block models.

As our widget will be driven by a JavaScript file we will also need to declare a JavaScript plugin for the page builder to automatically alias.

Here we have the example from the product search widget:
```
<config>
    ...
    <plugins>
        <js>
            ...
            <gene_widget_product_search>
                <gene_product_search_widget>
                    <alias>bluefoot/widget/search/product</alias>
                    <path>widget/core/search/product</path>
                </gene_product_search_widget>
            </gene_widget_product_search>
            ...
        </js>
    </plugin>
    ...
</config>
```

Within a JavaScript plugin you need to declare two attributes:

- **alias**: The alias which this file can be loaded from, in the case of all widgets this should start with `bluefoot/widget`. It should then be prepended by the alias of the widget declared earlier in the widgets section.
- **path**: The actual path to the JavaScript file, in the case of a core widget the file will be located in the core folder. However you should locate your file within the local directory. You don't include the .js extension with the path.

#### Other Resources
If your widget requires a 3rd party library or another JavaScript file as well as your widget file you'll need to implement another entry in `plugins/js`.

For instance the core upload widget has a depdency for DropzoneJs:
```
<gene_widget_upload>
    <gene_upload_dropzone>
        <alias>bluefoot/dropzone</alias>
        <path>resource/dropzone</path>
    </gene_upload_dropzone>
    <gene_upload_widget>
        <alias>bluefoot/widget/upload</alias>
        <path>widget/core/upload</path>
    </gene_upload_widget>
</gene_widget_upload>
```

Our JavaScript widget file then contains a depedency for `bluefoot/dropzone` which will automatically load Dropzone into our widget.

> These plugins don't currently support non-AMD compatible (RequireJS) JavaScript files.

#### JavaScript
Every widget has a JavaScript file that handles the functionality of the widget. This widget is responsbile to building the HTML elements, updating their values and can provide further functionality when serializing the edit form.

From our XML earlier we know that the product search widget is located at `widget/core/search/product`, this file is a standard RequireJS file with a class being created within. In this instance the product widget is in a sub folder of search, however this isn't a requirement but suggested to aid in the organisation of widgets.

Our actual widget file will start with RequireJS's standard define call, this will declare any dependencies that the widget has on other libraries, for instance we are requiring the configuration, jQuery and the Hook system.
```
/**
 * - Product.js
 *
 * @author Aidan Threadgold <aidan@gene.co.uk>
 */
define(['bluefoot/config', 'bluefoot/jquery', 'bluefoot/hook'], function (Config, jQuery, Hook) {

    /**
     * Our Field class
     *
     * @constructor
     */
    function InputField(field, value, edit) {
        this.field = field;
        this.value = value;
        this.element = false;
        this.entity = edit.entity;

        this.attachHooks();
    }
```

The constructor of the class will be passed 3 different parameters.

- **field** [object]: The fields information that is being rendered, this will include any information from the configuration call earlier.
- **value**: The previous value for this field, if the field has different make up from a standard field the widget will need to implement logic to correctly update it's components.
- **edit**: The instance of the edit class that was used to build this widget. This can be used when the widget needs to interact with other parts of Blue Foot.

> ***Abstract Class***: In a future version we will implement an abstract class that can be extended and provides basic functionality for functions that are commonly implemented.

The widget will then need to implement a `buildHtml` function which will return the fields HTML to be rendered into the edit form.

Again using our product search as an example, we can see the widget creating various HTML elements using jQuery, assigning classes and variables:
```
    /**
     * Build the dom element to represent this widget within the edit panel
     *
     * @returns object
     */
    InputField.prototype.buildHtml = function () {
        var id = this.getId(),
            fakeId = this.getId() + "_autocomplete",
            placeholder = '';

        // Get the product name for the placeholder text
        if (this.entity.data[this.field.code + "_product_name"]) {
            placeholder = this.entity.data[this.field.code + "_product_name"];
        }

        // Build elements
        this.element = jQuery('<div />').addClass('gene-cms-search-field gene-cms-input-field');
        this.element.attr("id", this.getId() + "_wrapper");
        this.element.append(
            // Append label
            jQuery('<label />').attr('for', fakeId).html(this.getLabel()),

            // Append an input we're going to use for searching
            jQuery('<input />').attr({
                name: this.field.code + "_product_name",
                type: this.getType(),
                id: fakeId
            }).val(placeholder),

            // Append the actual input the CMS uses for saving
            jQuery('<input />').attr({
                name: this.field.code,
                type: 'hidden',
                id: id
            }).val(this.value)
        );

        // Attach field note
        if (typeof this.field.note !== 'undefined') {
            this.element.append(jQuery('<p />').addClass('gene-cms-note').html(this.field.note));
        }

        return this.element;
    };
```
You can also observe the widget setting various elements to class variables to be later used and updated according to other functions within the widget. 

The output is buildHtml is called before the field is rendered onto the page, so if you require any JavaScript to be executed once the widget is present on the page you will need to implement an attachment to the hook `'gene-bluefoot-after-render-field-' + this.field.fieldType`. This is called after every field has been rednered, you can attach this hook by implementing the following in your construct, or in a called function (such as `attachHooks` in the core Blue Foot widgets and input fields).

```
Hook.attach('gene-bluefoot-after-render-field-' + this.field.fieldType, this.afterRenderField.bind(this));
```

This will call `onRenderField` once the system has built the field, please see the [hook documentation] for further information on how to use hooks correctly. 

As we have a single instance of this class we can utilise all the elements that have been set within the class earlier, for instance `this.element`. In the case of the product search we initialize a version of jQuery UI's auto complete.

```
    /**
     * After the field has been added to the dom, instantiate the jQuery autocomplete instance
     * @param $hook
     */
    InputField.prototype.afterRenderField = function ($hook) {
        this.element.find('input').first().autocomplete({
            source: Config.getPluginConfig('gene_widget_search_product', 'source_url'),
            appendTo: "#" + this.getId() + "_wrapper",
            minLength: 2,
            select: function (event, ui) {
                jQuery("#" + this.getId()).val(ui.item.id).data("product-name", ui.item.value);
                jQuery("#" + this.getId()).val(ui.item.id).data("product-name", ui.item.value);
            }.bind(this)
        });

        $hook.done();
    };
```

#### Plugin Config
Your widget may need to pass through further data from Magento the page builder instance. For instance a URL, system configuration settings or something that has to be generated server side. In this instance you can add information into the configuration via a Magento event.

For this you will need to implemenet an observer for ```gene_cms_stage_build_config```. This follows the same format as standard Magento events within your ```config.xml```. This event has one parameter ```config``` which contains the configuration that will be returned to the page builder.
```            
<gene_cms_stage_build_config>
    <observers>
        <your_module_widget_config>
            <type>singleton</type>
            <class>your_module/observer</class>
            <method>stageBuildConfig</method>
        </gene_cms_widget_upload>
    </observers>
</gene_cms_stage_build_config>
```

Within this event you will be able to modify the config array to contain further information that's required by your widget, for instance the product search widget includes an ajax endpoint for completing search operations:
```
    /**
     * Event called when we're building our stages config
     *
     * @param \Varien_Event_Observer $observer
     *
     * @return $this
     */
    public function stageBuildConfig(Varien_Event_Observer $observer)
    {
        /* @var $config Varien_Object */
        $config = $observer->getEvent()->getConfig();
        $pluginData = $config->getData('plugins');

        // Search URLs
        $pluginData['gene_widget_search_product']['config']['source_url'] = Mage::helper('adminhtml')->getUrl('adminhtml/stage_widget_search/search', array('context' => 'product'));
        
        $config->setData('plugins', $pluginData);

        return $this;
    }
```

See more on the page builders configuration [here].

As we're adding this key to the plugin data, under the ```gene_widget_search_product``` array we can access it later using the ```Config.getPluginConfig``` function, as you can see in the earlier example JavaScript.

```
Config.getPluginConfig('gene_widget_search_product', 'source_url')
````

#### Summary
Widgets are a great way to add new functionality into your extension to Blue Foot. They can drastically enhance a users experience through clever UX and customization of elements.


