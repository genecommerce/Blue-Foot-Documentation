# Blue Foot: Page Builder
## Walkthrough
The entirity of the Page Builders JavaScript code is located in `js/gene/bluefoot/` within this folder you will see a number of folders for the various different aspects of the page builder.

- `component` - houses the various components required by the system to initialize, interact with the server side and interact with the user.
- `content-type` - each content block can have a custom content block class assigned to it, the core only ships with an abstract content block class. This class handles the functionality associated with the particular content block and it's data.
- `resource` - contains any 3rd party resources that are required for the system to work, this is where you'll find the numerous jQuery libraries along with other libraries used throughout the page builder.
- `widget` - fields can have widgets assigned to them which can dramatically customize the functionality of the particular field, you can find out more about widgets [here](PageBuilder/Edit/Widgets.md).

### Core vs Local
You'll see within a number of directories there are both core, and local folders. Core folders are reserved for the core Blue Foot development team, any customizations or additions to the system should be created in the local folder, followed by a namespace. 
For instance creating a new local storage component you'd create the file `component/local/namespace/localstorage.js`.

If you're wanting to override a core file you're able to do this via our [plugin system](PageBuilder/Plugins.md).

> You shouldn't ever modify a core file, doing so will cause your changes to be removed when the module is updated. If you believe there is a bug, or a change needs to be made to the core files which cannot be achieved through the plugin system please contact us. 

### Components
These are the core backbone to the page builder, they provide different crucial parts of the application.  

- `ajax.js` - provides an easy to use Ajax wrapper to the jQuery ajax functionality. Can easily be swapped out in preference for another library, or a core JavaScript implementation.
- [`config.js`](PageBuilder/Component/Config.md) - handles the on page configuration, alongside making a request to the server to retrieve the full configuration, along with any entity data.
- [`dragdrop.js`](PageBuilder/Component/DragDrop.md) - provides an interface to jQuery UI's drag, drop and sort libraries.
- [`edit.js`](PageBuilder/Edit.md) - initializes and handles a user editing a content block.
- [`hook.js`](PageBuilder/Hooks.md) - our pub/sub system for attaching and triggering events.
- [`modal.js`](PageBuilder/Modal.md) - an alternative to browser alerts and confirms. Provides the user with helpful feedback without blocking their browsers functionality.
- [`plugins.js`](PageBuilder/Plugins.md) - the initialization and running of any defined plugins.
- `renderer.js` - an interface between the page builder and MustacheJS.
- [`stage.js`](PageBuilder/Stage.md) - every instance of the page builder is refered to as the stage, this file handles the communication between all the other components. A new version of the inner class is initialized when a user enables blue foot.
  - `build.js` - handles rebuilding the stages content blocks from a stored string.
  - `panel.js` - builds the left handle panel, handling the rendering of content blocks and initializing the drag drop class to provide drag and drop functionality.
  - `save.js` - utilises the hook system to automatically build the data into a JSON object that the server side can parse, and save. 
- `structural.js` - handles any static structural elements that can be added on the page, this currently includes rows and columns.
- [`template.js`](PageBuilder/Template.md) - provides functionality to allow users to save templates of their current page builder content, alongside reloading this.
