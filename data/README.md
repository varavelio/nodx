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
    "description": "Description of the attribute"
  },
  {
    "...": "..."
  }
]
```

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
