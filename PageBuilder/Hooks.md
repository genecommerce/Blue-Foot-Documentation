# Blue Foot: Page Builder
## Hooks

The hook system is a pub/sub, observer/event system built directly into the core of the page builder. This enables easy extension to numerous actions a user can make whilst interacting with the page builder. 

You can find the hook JavaScript at `js/gene/cms/component/core/hook.js` this is the first thing that is initialized in the page builder application. It's also heavily used by the core for updating various UI elements.

### Triggering an event
If you have created a widget, or custom plugin you may need the system to know you've updated something to re-trigger previews or rendering. You can do this with the following code:

```
Hook.trigger('gene-bluefoot-stage-ui-updated', false, false, this.stage);
```
When calling `Hook.trigger` there are 4 possible paramters, the first is already required.

- **name**: The event you wish to trigger.
- **params**: Any parameters you wish to be available to anything attached onto this event.
- **completeFn**: A function to be ran once the event has been fully ran.
- **origin**: The origin of the hook being trigger, it's good practice to pass the current stage into this parameter as the hook system is global.

### Attaching/observing an event
If you're wanting to conduct a certain action when the system triggers a certain event you can declare an attachment onto that events name. For instance the system uses these to tweak various parts of the UI when entities are updated.

```
Hook.attach('gene-bluefoot-stage-ui-updated', function ($hook) {
    // Logic here
    $hook.done();
}.bind(this));
```
Attaching onto an event only has two parameters, the first being the name of the event you're wanting to attach onto and the second being the function to call when the event is triggered.

The complete function will be passed an instance of `$hook` for the system to behave correctly you must **always** call `$hook.done();` once your processing is finished, failure to do so could cause some very strange behaviour.

> Failure to call $hook.done() could result in the page builder system failing to initialize or complete important operations. If you're building a system utilising hooks and experience an issue please ensure all your hooks are calling this once their processing has finished.

#### Asynchronous callback
If you need to conduct an asynchronous action (such as an Ajax request) you're able to call `$hook.done()` once that operation has completed or failed. The hook system will wait until the operation is completed, and the callback is called to run the next hook, or complete the hooks callback function.

```
Hook.attach('gene-bluefoot-stage-ui-updated', function ($hook) {
    var Ajax = new AjaxClass();
    Ajax.post('url', {}, function (data) {
        // Logic here from response
        $hook.done();
    });
}.bind(this));
```

> Not all triggered hooks wait for the attached hooks to finish, you can determine this by seeing if the Hook.trigger implements a callback function.

### Parameters
When parameters are passed during the trigger event they're available within the `$hook` object, under the `params` key.

```
Hook.trigger('example-hook', {message: 'hello'}, false, this.stage);
```

You could then access the message variable via
```
$hook.params.message;
```

### Core hooks
The core implements a number of different hooks to allow your plugin / widget to interact with many different parts of Blue Foot.

- gene-bluefoot-stage-ui-updated
- gene-bluefoot-stage-restore-empty-text
- gene-bluefoot-stage-updated
- gene-bluefoot-stage-visible
- gene-bluefoot-entity-update-previews
- gene-bluefoot-after-edit-view
- gene-bluefoot-after-edit-init
- gene-bluefoot-save-configure-entity
- gene-bluefoot-validate-form
- gene-bluefoot-before-render-field
- gene-bluefoot-after-render-field-*FIELD_TYPE*
- gene-bluefoot-plugins-prepare-before
- gene-bluefoot-plugins-prepare-paths
- gene-bluefoot-plugins-prepare-after
- gene-bluefoot-before-stage-init
- gene-bluefoot-after-stage-init
- gene-bluefoot-build-complete
- gene-bluefoot-panel-get-config
- gene-bluefoot-stage-save-extra
- gene-bluefoot-get-options-*CODE*

> If you believe we should implement further hooks throughout the system please contact us and we will include these in future releases.

### Triggering your own hook
We strongly suggest implementing your own hooks, so others can inject functionality into your code.

#### Waiting hook
You can implement a hook so that it won't run the callback functionality until all attached hooks have finished running. You can see a number of these being called in the early initializing of the page builder.

Here you can see an example from `stage.js`
```
// Fire a trigger
Hook.trigger('gene-bluefoot-before-stage-init', {
    button: jQuery(button),
    id: this.id,
    container: container
}, function () {
    // Logic to run after hook
}.bind(this), this);
```

#### Non waiting hook
A non waiting hook will call the attached events, but won't wait until they're completed to continue on with the rest of the functionality. You can implement a non waiting hook by passing false in place of the callback function.

Here you can see how we inform the system that the stages UI has been updated. This runs functionality to ensure the stage is being presented correctly.
```
Hook.trigger('gene-bluefoot-stage-ui-updated', false, false, this);
```
