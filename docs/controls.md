## Controls

A control is a little snippet of JSON that provides Frontender Desktop with instructions for rendering a form element. Frontender has a few core controls that you can use, and you can also create new controls. You can create new controls from scratch, or you can re-purpose existing controls.


Example of a control definition:

```JSON
{
    "text": {
        "value": "My text",
        "type": "text",
        "label": {
            "en-GB": "Text"
        },
        "control": "core/abstract",
    }
}
```

### Control Types

| Name | Description |
| ---- | ---- |
| `text` | A single line of text input |
| `textarea` | A larger text field |
| `number` | A number input field |
| `list` | A dropdow list |
| `boolean` | A switch or radio button |
| `compound` | A combination of one or more other controls |
| `lookup` | A compounded selector for external content |
| `hidden` | A hidden input will not render on-screen |

### Advanced Properties

| name | Value | Description |
| ----  | ---- | ---- |
| `repeatable` | `boolean` | Enables/Disables repetetion |
| `at_least` | `number` | Only applies to repeatable controls. Specifies the minimum number of repetions required. Omit when not needed. |
| `at_most` | `number` | Only applies to repeatable controls. Specifies the maximum number of repetions allowed. Omit when not needed. |
| `translatable` | `boolean` | Defaults to `true`. When `false`, the value can not be translated. |


### Core Controls

Core controls are pre-configured controls that can be used in your projects:

| Name | Description |
| ---- | ---- |
| `core/abstract` | A base control from which each other control is extended. |
| `core/text` | A single line text input field. |
| `core/textarea` | A larger text field, the rows attribute will control the initial field height. |
| `core/markdown` | Like core/textarea, for markdown styling. |
| `core/json` | Like core/textarea, for JSON objects. |
| `core/list` | A configurable dropdown list. |
| `core/number` |  A number input field. |
| `core/boolean` | A dropdown list with two options ("Yes" and "No"). Default value is "No". |
| `core/domain` | A compounded field, but with a prepended protocol dropdown list. Use it to simplify url entries. |
| `core/page` | A dropdown list with a list of published pages in the project. |
| `core/route` | A compounded field that can be configured to any link, including external links. Can be supplied with a resource id to link to dynamic resources (for instance a blog article or a product page). |
| `core/robots` | A dropdown list with indexing instructions for search engines. |


#### Overriding Controls

All properties can be manipulated by adding overriding instructions. This can be used for something simple like changing the controls label, or for more advanced overrides; like supplying a different list of options to a dropdown list.

Following is an example of an override to the `core/boolean` control:

```JSON
{
    "control": "core/boolean",
    "label": {
        "en-GB": "My different label"
    },
    "options": [
        {
            "value": "1",
            "label": {
                "en-GB": "Show this thing"
            }
        },
        {
            "value": "0",
            "label": {
                "en-GB": "Hide this thing"
            }
        }
    ]
}
```

#### Advanced controls

##### Compound controls

Controls can be combined into more complex, compounded controls. An example of a compound control is the `core/route` control, that combines three `core/text` fields:
```JSON
{
    "label": {
        "en-GB": "Page"
    },
    "type": "compound",
    "controls": {
        "label": {
            "label": {
                "en-GB": "Label"
            },
            "control": "core/text"
        },
        "page": {
            "label": {
                "en-GB": "Page"
            },
            "control": "core/text"
        },
        "id": {
            "label": {
                "en-GB": "Resource ID (optional)"
            },
            "control": "core/text"
        }
    },
    "control": "core/abstract"
}
```

##### Repeatable controls

Each control can be made repeatable by adding the `"repeatable": true.` attribute. A minimum and maximum number of repetitions gives further control on how the control may be used. Compound controls are also repeatable.

```JSON
{
    "label": {
        "en-GB": "Body text"
    },
    "control": "core/markdown",
    "repeatable": true,
    "at_least": 1,
    "at_most": 10
}
```

### Importing Controls

Create a file for each control. Place the file(s) in `/project/db/controls/…/controlname.json`. Any additional folder structure wil result in additional namespacing for the imported control.

For example, `/project/db/controls/theme/colour.json`:
```JSON
{
    {
        "label": {
            "en-GB": "Theme Colour"
        },
        "value": "",
        "control": "core/list",
        "options": [
            {
                "label": {
                    "en-GB": "Red",
                    "value": "#FF0000"
                },
                "label": {
                    "en-GB": "Green",
                    "value": "#008000"
                },
                "label": {
                    "en-GB": "Blue",
                    "value": "#0000FF"
                }
            }
        ]
    }
}
```

Used in a blueprint configuration:
```JSON
{
    "theme": {
        "value": "#FF0000",
        "control": "project/theme/colour"
    }
}
```

The controls are imported by running the following from the command line:
```
composer run-script import-project-controls
````

This makes re-use of commonly applied controls much easier.