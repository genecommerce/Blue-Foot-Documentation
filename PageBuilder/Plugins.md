# BlueFoot: Page Builder
## Plugins
You can declare a plugin to run specific code at a certain point within the page builder. This can be useful to attach a number of hooks, fire other hooks or modify aspects of the stage.

### Declaring a plugin
We declare our plugin via the `plugins` section of the `pagebuilder.xml`. You can see the core module utilises this functionality often to include various aliases for widgets and other components.

To create a new plugin you would declare the following
```
<plugins>
    ...
    <js>
        ...
        <gene_example>
            <alias>example</alias>
            <path>component/local/gene/example/plugin</path>
            <load>onPageLoad</load> <!-- options are onPageLoad, onStageInit -->
        </gene_example>
        ...
    </js>
    ...
</plugins>
```

- **alias**: How the JavaScript file can be called as a dependency in RequireJS.
- **path**: The path to the JS file, as you're creating a new custom resource this should reside in the local folder.
- **load** (optional): At which point should the system initialize your file. This can be either on the page load (`onPageLoad`) or when the stage is initialized (`onStageInit`).

For the example above we must create `plugin.js` within the `local/gene/example` component folder.
```
/**
 * Example Plugin for BlueFoot
 *
 * @author Dave Macaulay <dave@gene.co.uk>
 */
define(['hook'], function (Hook) {

   // Declare any events
   Hook.attach('gene-bluefoot-before-stage-init', function ($hook) {
      // Do various logic
      $hook.done();
   });

   return {
       /**
        * Initialize function called by the CMS
        *
        * @param stage
        */
      init: function (stage) {
         // Do any init logic
      }
   };
});
```

Here you can see we're attaching a hook that will be triggered when any stage is initialized. We are passed the stage within the init function if it's declare. If you only want to attach a hook to the specific stages instead of globally you would need to move your hook into the init function and pass the stage as the context appropriately ([see hooks](Hooks.md)).

### jQuery plugins
As you may of seen whilst inspecting the code there is also a `jquery` tag defined with a number of child entities. You can find out more about how to use this functionality within the resources section.

[Read more about jQuery plugins](Resources.md#including-your-own-jquery-plugin)

### Async
The final section of the plugins area is the async area. This is used for async dependency load like JSONP and Google Maps. Within the core module this is used to ensure the correct loading of the Google Maps API.

```
<plugins>
    ...
    <async>
        ...
        <google_maps>
            <path>//maps.google.com/maps/api/js</path>
        </google_maps>
        ...
    </async>
    ...
</plugins>
```

All async plugins are loaded at the start of a stage being initialized and will be available throughout the application.
