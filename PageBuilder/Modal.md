# Blue Foot: Page Builder
## Modal
The modal system allows the system to present the user with attractive and user orientated messages.

The functionality for the modal system can be found within the `modal.js` component.

### Calling the modal system
The modal system is included early on in the applications initializing, this means you don't have to declare it as a dependency of the current file.

To retrieve an instance of the modal file, you can call:
```
require('bluefoot/modal');
```

However we would suggest you declare this as a depedency of the current file so it can be used multiple times. If you declare it as a depdency instead of using a require, you will use the variable you assign the instance to in place of the `require('bluefoot/modal')` code above.

```
define(['bluefoot/hook', 'bluefoot/modal'], function (Hook, Modal) {
    ...
    Modal.alert(..);
    ...
});
```

### Alert
If you're wanting to fire an alert you can do so by calling the following.

```
require('bluefoot/modal').alert('Your alert message', 'Window Title');
```

Replacing the alert message, and window title with your prefence, you can ommit the window title and the system will fallback to the default.

### Confirm
You can also fire a confirmation box requesting the users interaction.

```
require('bluefoot/modal').confirm('Confirmation Message', 'Window Title', function () {
    // The user has accepted the confirmation window
}.bind(this));
```

Again replacing the message and title with your preference. The callback function will be ran if the user accepts the confirmation window.
