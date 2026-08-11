# Data

Here are stored the static data used by the project implementations.

This data is useful if you are creating a NodX implementation and want to use
code generation to avoid repeating the boilerplate, as you can create some small
primitives and write a script that, based on the data in this folder, generates
the rest of your implementation programmatically.

## [elements.json](elements.json)

This file contains the list of HTML elements included in the project.

### Shape

```json
[
  {
    "name": "element",
    "isVoid": false,
    "description": "Description of the element"
  },
  {
    "...": "..."
  }
]
```

## [attributes.json](attributes.json)

This file contains the list of HTML attributes included in the project.

### Shape

```json
[
  {
    "name": "attribute",
    "isBoolean": true,
    "description": "Description of the attribute"
  },
  {
    "...": "..."
  }
]
```

The `isBoolean` field is only present (`true`) on attributes that the HTML
standard defines as boolean attributes: they take no value and simply appear or
disappear, so they render as just the attribute name (or are omitted entirely).
Attributes that look boolean but accept a value or keyword (e.g. `hidden`,
`download`, `capture`, `popover`) are not marked.

## [keywords.json](keywords.json)

This file contains the list of keywords marked as conflictive in the project

### Shape

```json
{
  "Go": ["keyword", "keyword", "..."],
  "JavaScript": ["keyword", "keyword", "..."],
  "...": ["..."]
}
```
