# Data

Here are stored the static data used by the project.

## [elements.json](elements.json)

This file contains the list of HTML elements included in the project.

Useful for code generation of the NodX template engine across multiple
languages.

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

Useful for code generation of the NodX template engine across multiple
languages.

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

This file contains the list of keywords marked as conflictive in the project.

Useful to determine conflicting keywords when generating code for the NodX
template engine across multiple languages.

### Shape

```json
{
  "Go": ["keyword", "keyword", "..."],
  "JavaScript": ["keyword", "keyword", "..."],
  "...": ["..."]
}
```
