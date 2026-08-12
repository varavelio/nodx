<h1 align="center">NodX</h1>

<p align="center">
  <em>Build HTML with the full power of your own programming language.</em>
</p>

<p align="center">
  <a href="LICENSE">
    <img src="https://img.shields.io/github/license/varavelio/nodx.svg" alt="License"/>
  </a>
  <a href="https://github.com/varavelio/nodx">
    <img src="https://img.shields.io/github/stars/varavelio/nodx?style=flat&label=github+stars"/>
  </a>
</p>

<p align="center">
  <a href="https://varavel.com">
    <img src="https://cdn.jsdelivr.net/gh/varavelio/brand@1.0.0/dist/badges/project.svg" alt="A Varavel project"/>
  </a>
</p>

## What is NodX?

NodX is a cross-language HTML template engine. Instead of learning yet another
template syntax, you build HTML with the plain functions of your favorite
programming language:

```go
nodx.Div(
  nodx.Class("card"),
  nodx.H1(nodx.Text("Hello, NodX!")),
)
```

No strings, no magic, no DSL. Everything is a node, and a final `render` step
turns the tree into an HTML string.

## What is the goal?

One idea, every language. This repository holds the single
[specification](SPEC.md) and the shared [data files](data/) (every HTML element
and attribute) that all implementations are built from, so NodX feels the same
whether you write Go, TypeScript, Python or anything else. Learn it once, use it
everywhere.

## Why NodX and not a template engine or a JS framework?

Generating HTML on the server usually means choosing between two options, and
both leave a gap:

- **Template engines** (Jinja, Blade, EJS...) are simple, but HTML lives in
  strings with magic syntax: no type safety, no autocomplete, and a
  mini-language to learn.
- **JavaScript frameworks** (React, Vue, Svelte...) are powerful, but they are
  built for interactive client-side apps. Virtual DOM, hydration, state, build
  steps and bundles are a lot of machinery when all you need is `<div>`s on the
  server.

NodX fills that gap:

|                        | NodX                                                   | Template engine               | JS framework                       |
| ---------------------- | ------------------------------------------------------ | ----------------------------- | ---------------------------------- |
| You write HTML with    | native functions (`Div`, `Class`...)                   | strings + `{{ }}` syntax      | JSX / JavaScript                   |
| Type safety            | full - the compiler catches typos                      | none                          | partial                            |
| Runtime in the browser | none                                                   | none                          | yes - bundle, hydration            |
| New syntax to learn    | none                                                   | a mini-language               | JSX + framework concepts           |
| Built for              | server-rendered HTML, simple UIs, static sites, emails | classic server-rendered pages | rich, interactive client-side apps |

If your job is producing HTML (e.g. a blog, a landing page, an email, a
server-rendered dashboard) NodX gives you the safety of a framework with none of
the baggage. And if you are building a real-time, state-heavy app, use a
framework: NodX is not here to replace React, just to make writing HTML feel
like writing code again.

## Implementations

| Language | Implementation                                |
| -------- | --------------------------------------------- |
| Go       | [nodxgo](https://github.com/varavelio/nodxgo) |

Your language missing? Build one from the [specification](./SPEC.md) and open a
PR to add it to the list, NodX is quite simple, you'll enjoy building it!

## Ecosystem

Each implementation can grow its own ecosystem of ready-to-use libraries. Here
are the projects built on top of each one:

### Go

- [**nodxgo-alpine**](https://github.com/varavelio/nodxgo-alpine) — Alpine.js
  attributes for NodX Go.
- [**nodxgo-htmx**](https://github.com/varavelio/nodxgo-htmx) — HTMX attributes
  and server utilities for NodX Go.
- [**nodxgo-lucide**](https://github.com/varavelio/nodxgo-lucide) — Beautiful &
  consistent icons for NodX Go, provided by [Lucide](https://lucide.dev/).
- [**nodxgo-simpleicons**](https://github.com/varavelio/nodxgo-simpleicons) —
  The [Simple Icons](https://simpleicons.org/) brand icons set for NodX Go.

> **Building your own NodX library?** We'd love to feature it here! Open a pull
> request adding your project under your language and help grow the NodX
> ecosystem together.

## License

MIT. See [LICENSE](LICENSE) for details.
