# Blue Foot: Page Builder
## Resources
Blue Foot is bundled with a number of resources that are required for various operations and user interactions within the page builder. You're welcome to call any of these as dependencies in your own widgets & plugins.

These resources are located within the `resource` JS directory. They are organised into sub-folders if there are multiple resources under a similar name or type. There maybe both minified and unminfied versions of various resources bundled in the module, below we will list the unminified versions for reference.

#### RequireJS [requirejs.org](http://requirejs.org)
`require.js`
> This is automatically loaded globally to handle dependencies within the page builder. 

[Read more on Overview](PageBuilder/Overview.md#requirejs)

#### jQuery [jquery.com](http://jquery.com)
`jquery-1.11.3.min.js`

#### BlueFoot jQuery Extensions
`jquery.bluefoot.normaliseHeights.min.js`
`jquery/bluefoot-accordion/jquery.bluefoot.accordion.js`
`jquery/bluefoot-normalise-heights/jquery.bluefoot.normaliseHeights.js`
`jquery/bluefoot-tabs/jquery.bluefoot.tabs.js`

We have built numerous jQuery extensions to provide various functionality for dynamic widgets and data.

#### jQuery UI [jqueryui.com](http://jqueryui.com)
`jquery-ui/jquery-ui.min.js`

We implement numerous interactions from the jQuery UI library to ensure compatibility with all browsers. This includes drag & drop and sortable.

#### FancyBox 2 [fancyapps.com/fancybox](http://fancyapps.com/fancybox/)
`jquery/fancybox/jquery.fancybox.js`

We use FancyBox 2 to power our lightboxes.

#### Slick [kenwheeler.github.io/slick](http://kenwheeler.github.io/slick/)
`jquery/slick/slick.js`

We use Slick for any carousels that are rendered within the preview view in the admin, or on the front-end.

#### TagIt [aehlke.github.io/tag-it](http://aehlke.github.io/tag-it/)
`jquery/tag-it/tag-it.js`

Tag it is used within the `tags.js` widget to provide better usability when entering a list of options.

#### Dropzone.js [dropzonejs.com](http://www.dropzonejs.com)
`dropzone.js`

Dropzone is use for any image and asset uploading within the BlueFoot system.

#### HTML2Canvas [html2canvas.hertzen.com](https://html2canvas.hertzen.com)
`html2canvas.js`

We render the stage into a PNG file to be stored alongside templates using HTML2Canvas.

#### MustacheJS [github.com/janl/mustache.js](https://github.com/janl/mustache.js/)
`mustache.min.js`

Mustache is a powerful logicless template rendering system. It allows us to declare our templates and render them using objects.

### Including your own jQuery plugin
There maybe a need to include your own jQuery plugin for use within a widget or plugin. To do so you'll need to ensure that the plugin is loaded within our `bluefoot/jquery` no conflict version. This is to ensure we don't pollute any global jQuery instances with the additions the page builder system makes.

Within the `plugins` section of the pagebuilder.xml you will see a number of declare jQuery plugins.
```
<plugins>
    ...
    <jquery>
        ...
        <jquery_tag_it>
            <alias>jquery/tag-it</alias>
            <path>resource/jquery/tag-it/tag-it.min</path>
            <deps>
                <jquery_ui>bluefoot/jquery/ui</jquery_ui>
            </deps>
        </jquery_tag_it>
        ...
    </jquery>
    ...
</plugins>
```
As you can see above for the jQuery tag it plugin which is used within the tags widget. It declares the alias of the plugin, the path to that plugin and any depdencies that plugin has.

This will be loaded into the page builder system, and then become available via the alias.

So in the case above you'd put `jquery/tag-it` as a depdency within the widget or plugin you need to call it within. You'd also in this instance need to have `bluefoot/jquery/ui` and `bluefoot/jquery`.


