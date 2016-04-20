# BlueFoot: Page Builder Blocks
## Input Types
Magneto implments a number of familiar input types, along with renderers within their form widgets. Our page builder implements these input types in JavaScript to align the page builder experience with the true capabilities of Magento.

The input type classes are located within `component/core/edit/fields`. All input types and widgets extend the abstract class located within `abstract.js`.

- `date.js`: Provides Magento date picker functionality.
- `select.js`: Provides select and multi select functionality.
- `text.js`: Provides simple text inputs.
- `textarea.js`: Provides textarea functionality.

Dynamic field types, such as the WYSIWYG editor, are driven by [widgets](Widgets.md)

The system maps the Magento input fields within the `Gene_BlueFoot_Model_Stage_Data` class and the `_flattenAttributeData` function.
