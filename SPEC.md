# The NodX Specification

NodX is a cross-language HTML template engine. The idea is simple: instead of
learning a template syntax, you build HTML with the plain functions of your own
programming language. No strings, no DSL, no magic, just code.

This document is the contract that every NodX implementation should follow. If
you are building a NodX library for your favorite language, this is your guide.

## The big idea

Most template engines work like this: you write HTML with a special syntax
(`{{ }}`, `{% %}`, `<% %>`...) and the engine parses it at runtime. That means
two languages in one file, no autocomplete, no type safety, and a debugging
session every time you mistype a variable name.

NodX flips the script. The programming language _is_ the template language. HTML
elements and attributes become plain functions (e.g. `Div()`, `Class()`,
`Img()`) and you compose them like any other data structure. The result is a
tree of nodes that you can inspect, reuse, and finally `render` into an HTML
string.

## Where NodX fits in the web ecosystem

Server-side web development has settled into two camps, and both leave a gap
that NodX fills.

**Template engines** (e.g. Jinja, Blade, EJS, Handlebars) are the classic
answer. They are simple and battle-tested, but HTML lives in strings with a
special syntax. Your editor cannot check or format it, your compiler cannot
check it, and the only feedback you get is at runtime, if you are lucky.

**JavaScript frameworks** (e.g. React, Vue, Svelte) are the modern answer. They
are incredibly powerful for interactive applications, but that power comes with
a price: a runtime in the browser, a virtual DOM, hydration, state management, a
build step, and usually a second language in your project just to print
`<div>`s.

|                          | NodX                                                   | Template engine                    | JS framework                                          |
| ------------------------ | ------------------------------------------------------ | ---------------------------------- | ----------------------------------------------------- |
| HTML is written with     | native functions (`Div()`, `Class()`)                  | template files with `{{ }}` syntax | JSX / JavaScript                                      |
| Type safety              | full - the compiler validates every tag and attribute  | none - typos are runtime surprises | partial - JSX, but still JavaScript                   |
| New syntax to learn      | none                                                   | a mini template language           | JSX plus framework concepts (state, hooks, lifecycle) |
| Runtime and dependencies | zero - pure functions                                  | a template parser                  | virtual DOM, hydration, bundles, build step           |
| Rendering happens        | on the server, in your language                        | on the server                      | in the browser (SSR exists, but adds machinery)       |
| Interactivity            | none - HTML only, you wire the JS yourself             | none                               | first-class - state, events, effects                  |
| Best for                 | server-rendered HTML, simple UIs, static sites, emails | classic server-rendered pages      | rich, interactive client-side applications            |

### The gap NodX fills

A huge share of web development is not interactive apps, it is producing HTML on
the server: pages, dashboards, emails, reports, static sites. For that job,
template engines are unsafe and frameworks are overkill. NodX targets exactly
that middle ground: the type safety of a framework and the simplicity of a
template engine, with zero runtime and 100% of its focus on HTML.

If your backend is already Go, Python, Rust or any other language, NodX lets you
generate HTML in that language - no second language, no build step, no runtime
in the browser.

### When NodX is not the answer

Be honest about the trade-offs:

- **Interactive apps.** Live updates, complex client state, real-time
  collaboration, use a JavaScript framework. NodX is not a React killer; it
  competes with the idea that you need React to write HTML.
- **HTML files maintained by non-programmers.** If designers edit the markup
  directly, a template engine with HTML-looking files may serve you better.
- **Legacy codebases.** If a template engine is already working for you,
  migrating is a decision, not a requirement.

## Core concepts

### Everything is a node

The whole engine is built around a single rule:

> **Everything is a node.**

There are exactly three kinds of nodes:

| Node           | What it represents             | Example                |
| -------------- | ------------------------------ | ---------------------- |
| Element node   | An HTML element                | `div`, `img`, `button` |
| Attribute node | An HTML attribute              | `class`, `src`, `href` |
| Text node      | Text content inside an element | `"Hello, world!"`      |

Elements contain other nodes: attributes describe them, text and other elements
live inside them. That's it, there is nothing else to learn.

### Build, then render

Working with NodX always has two phases:

1. **Build** - create nodes and nest them however you like.
2. **Render** - call `render()` on the root node to get the final HTML string.

The build phase is where you can do anything your language allows: loops,
conditionals, helper functions, your own components. The render phase is a
boring, predictable walk over the tree.

## The API (pseudo-code)

The exact spelling depends on the language, but every implementation must offer
the same building blocks.

### Elements

Elements are functions named after HTML tags, in PascalCase (if the language
allows it): `div` becomes `Div`, `img` becomes `Img`, and so on. They take a
variable list of arguments (attributes, children or both) in any order.

```pseudo
Div(attributes..., children...) -> ElementNode
Img(attributes..., children...) -> ElementNode
```

### Attributes

Attributes are functions named after HTML attributes, also in PascalCase (if the
language allows it), and take exactly one argument: the value.

```pseudo
Class("card") -> AttributeNode
Src("/img/avatar.png") -> AttributeNode
```

**Boolean attributes** (like `checked`, `disabled`, `required`) deserve a
special mention. When their value is boolean `true`, they render as just the
attribute name (e.g.`<input disabled>`). When `false`, they are omitted
entirely.

The reference data (`./data/attributes.json`) marks every attribute that HTML
defines as boolean with `"isBoolean": true`, so implementations can rely on that
flag instead of maintaining their own list.

### Text

Text nodes hold content and are **always escaped by default**. If you ever need
to inject raw HTML (e.g. output from a trusted markdown renderer) the
implementation should offer an explicit escape hatch (a `Raw` or `UnsafeHTML`
function) that makes it obvious you are bypassing safety.

```pseudo
Text("<b>bold</b>")   # renders as &lt;b&gt;bold&lt;/b&gt;
Raw("<b>bold</b>")    # renders as <b>bold</b>, use with care!
```

### Render

```pseudo
node.render() -> string
```

Rendering is recursive: an element renders its opening tag, its attributes (in
insertion order), its children and its closing tag (unless it is a void
element).

## A worked example

Say we want an avatar component: a container with an image inside.

```pseudo
avatar = Div(
    Class("avatar-container"),
    Img(
        Class("avatar-image"),
        Alt("Avatar"),
        Src("avatar.png")
    )
)

output = avatar.render()
```

Which produces:

```html
<div class="avatar-container">
  <img class="avatar-image" alt="Avatar" src="avatar.png" />
</div>
```

> Note: The indentation above is just for readability. Implementations may
> pretty-print or not, treat whitespace in the output as an implementation
> detail, never as part of the contract.

## Rules every implementation must follow

These are the non-negotiables. Skip any of them and you are not NodX:

1. **Void elements have no closing tag.** `img`, `br`, `input` and friends
   render as `<img ...>`. Self-closing syntax (`<img ... />`) is allowed but
   optional. The full list lives in the reference data (`./data/elements.json`).
2. **Escape by default.** Attribute values and text content must be escaped to
   protect users from XSS. Raw HTML is opt-in only.
3. **Boolean attributes render without a value.** `Disabled(true)` → `disabled`,
   `Disabled(false)` → nothing. The attributes HTML treats this way are marked
   with `"isBoolean": true` in `./data/attributes.json`.
4. **Keep insertion order** of attributes and children.
5. **Resolve keyword collisions.** If an element or attribute name collides with
   a keyword of the target language, pick a deterministic escape (like a suffix,
   e.g. `title` → `TitleEl`) and document it (see `./data/keywords.json`).
6. **Support custom components.** Users must be able to write functions that
   return a node tree. That is how component libraries are born.
7. **Maintain a simple and consistent API.** All NodX implementations should be
   small, including only the things a user might need to build their HTML
   templates, and should avoid adding unnecessary ceremony or functionality.
   Helpers should be minimal, universal, genuinely useful, and without
   unnecessary abstractions.

Implementations are free to add conveniences on top (conditional rendering,
class maps, loops, you name it) as long as these rules hold and the
implementation is simple enough.

## Reference data

This repository ships three JSON files that are the source of truth for code
generators (useful to generate the implementation's boilerplate). If you are
implementing NodX, build your language bindings from these files; do not invent
your own lists.

| File                   | Contents                                                                                           |
| ---------------------- | -------------------------------------------------------------------------------------------------- |
| `data/elements.json`   | Every HTML element, with its `isVoid` flag and a short description.                                |
| `data/attributes.json` | Every HTML attribute (including event handlers), with a short description and an `isBoolean` flag. |
| `data/keywords.json`   | Reserved words per language, to detect naming collisions during code generation.                   |

The `isBoolean` flag in `data/attributes.json` is present (`true`) only on
attributes that the HTML standard itself defines as boolean attributes: they
take no value and merely appear or disappear, so they render as just the
attribute name or are omitted entirely. Attributes that merely look boolean but
can carry a value or keyword are intentionally **not** marked - e.g. `hidden`
(an enumerated attribute that also accepts `until-found`), `download` (accepts a
filename), `capture`, `popover` and `sandbox` (keyword/token values).

> Note: If you notice any inconsistencies, missing or extra data in these files,
> an issue or pull request would be greatly appreciated to help keep the
> ecosystem as polished as possible.

## Why this approach?

- **Simplicity.** One concept (the node), a handful of functions. Nothing else
  to learn. If you know your language and HTML, you already know NodX and how to
  implement it if it doesn't already exist for your language.
- **Language-agnostic.** Any language that can express a function can express
  NodX.
- **Your language's full power.** Loops, conditionals, types, tests, all
  available while building templates.
- **No template syntax.** No parser, no preprocessor, no new tooling.
- **Composable.** Components are just functions that return nodes. Build a
  design system the same way you build any other library.
