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

## Implementations

| Language | Implementation                                |
| -------- | --------------------------------------------- |
| Go       | [nodxgo](https://github.com/varavelio/nodxgo) |

Your language missing? Build one from the spec and open a PR to add it to the
list!

## License

MIT. See [LICENSE](LICENSE) for details.
