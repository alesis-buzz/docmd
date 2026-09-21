# docmd

DocMD is a simple, lightweight documentation system for static sites, built
entirely around Markdown.

- One JSON file defines the navigation.
- Markdown files are fetched and rendered at runtime.
- No build step, no framework, no dependencies to install.
- Sidebar search filters sections, pages and links as you type.
- Skeleton loading state while a page is fetched.
- Syntax highlighting for code fences, loaded on demand.
- Copy button on every code block.
- Dark monochrome theme by default, light theme optional, accent color
  configurable.

## Usage

```html
<link rel="stylesheet" href="docmd.css">
<div id="docmd"></div>
<script src="docmd.js"></script>
<script>
  docmd.InitDocs("docs.json");
</script>
```

Or let the script initialize itself:

```html
<div id="docmd"></div>
<script src="docmd.js" data-docmd="docs.json"></script>
```

`docmd.css` is optional: if it is not found, DocMD injects its styles.

## Definition

```json
{
  "name": "DocMD",
  "primary": "#7aa2f7",
  "theme": "dark",
  "base_url": "docs/",
  "sections": [
    {
      "type": "section",
      "name": "Guide",
      "pages": [
        { "type": "page", "name": "Getting started", "md_url": "getting-started.md" },
        { "type": "link", "name": "GitHub", "url": "https://github.com" }
      ]
    }
  ]
}
```

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `name` | string | `Documentation` | Brand shown in the sidebar. |
| `primary` | string | text color | Accent color for links and active items. |
| `theme` | string | `dark` | `dark` or `light`. The user toggle is remembered. |
| `base_url` | string | location of the JSON | Directory for relative `md_url` values. |
| `sections` | array | required | `items` is accepted as an alias, sections can nest. |

`InitDocs` accepts an array, an object, a JSON string or a URL, plus an options
object:

```js
docmd.InitDocs("docs.json", { target: "#docmd", primary: "#7aa2f7", theme: "light" });
```

Options win over the JSON values. `target` accepts a selector or an element and
defaults to the element with id `docmd`, then `document.body`. Pass
`highlight: false` to disable code highlighting and `copy: false` to remove the
copy buttons.

## Routing

Pages use the hash route `#/slug`, so static hosts (GitHub Pages, Netlify, S3)
need no rewrite rules. Links between Markdown files are intercepted and routed
in place. Slug defaults to `name` lowercased and hyphenated.

## API

| Member | Description |
| --- | --- |
| `docmd.InitDocs(input, options)` | Mounts the documentation. Returns a Promise. |
| `docmd.setTheme("dark" \| "light")` | Switches the theme. |
| `docmd.setPrimary(color)` | Overrides the accent color. |
| `docmd.go(slug)` | Navigates to a page. |
| `docmd.getConfig()` | Returns the normalized configuration. |
| `docmd.version` | Current version. |

## Example

The repository root is a working example:

```
index.html
docs.json
docs/
  getting-started.md
  configuration.md
  markdown.md
docmd.css
docmd.js
```

Serve the folder over HTTP (`npx serve .`) and open `/`. It also deploys to
GitHub Pages as is, because every path is relative to the root.

## Markdown

Rendered with [marked](https://marked.js.org), loaded from jsDelivr the first
time a page renders. If you prefer to pin your own copy, include it before
`docmd.js` and DocMD will use it. Raw HTML in Markdown is allowed, except for
script tags and event handler attributes, which are stripped.

Code fences are highlighted with [highlight.js](https://highlightjs.org),
loaded from jsDelivr only when a page contains code. Include your own copy
before `docmd.js` to pin a version, or disable highlighting with
`{ highlight: false }` / `data-highlight="false"`. Token colors adapt to the
dark and light themes.

## License

MIT
