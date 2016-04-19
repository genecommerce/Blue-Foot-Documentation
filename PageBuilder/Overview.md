# Blue Foot: Page Builder
## Overview
The page builder provide drag and drop, sorting and editing functionality to any WYSIWYG editor within the Magento admin. It does this by including an 'Enable Blue Foot' button next to every WYSIWYG editor in the admin.

Upon pressing this button you will be presetened with the Page Builder UI. The UI is split into two sections, the left being the panel and the right being the stages contents. Each instance of the page builder is refered to as a stage.

### Technologies
We have included a number of technologies within the page builder system:

- [jQuery](http://jquery.com)
- [jQuery UI](https://jqueryui.com)
- [DropzoneJS](http://www.dropzonejs.com)
- [RequireJS](http://requirejs.org)
- [MustacheJS](https://github.com/janl/mustache.js)

These are all included using RequireJS, and will not pollute the window, so you're safe to use any other libraries outside of our systems without causing conflict.

### jQuery
jQuery could be considered an unpopular solution for many developers, however we made the decision to use jQuery due to it's great browser support and for the interactions jQuery UI provides. When performance is critical we only use native JavaScript code to ensure our solution isn't going to cause performance issue for the end user.

### RequireJS
RequireJS is a core component within the page builder and is used for all dependency loading, if you're planning on customization a part of the page builder you'll need to have a basic understanding of RequireJS.

> RequireJS is a JavaScript file and module loader. It is optimized for in-browser use, but it can be used in other JavaScript environments, like Rhino and Node. Using a modular script loader like RequireJS will improve the speed and quality of your code.

### MustacheJS
Provides a logicless templating system in JavaScript. Allows us to define templates and build them easily using objects.
