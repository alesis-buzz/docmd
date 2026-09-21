# Configuration

The definition passed to `docmd.InitDocs` can be an object, a bare array of
sections, a JSON string or the URL of a JSON file.

## Object form

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

## Keys

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `name` | string | `Documentation` | Brand shown in the sidebar. |
| `primary` | string | theme text color | Accent color for links, active items and focus states. Any CSS color except named colors. |
| `theme` | string | `dark` | `dark` or `light`. A user toggle is stored in `localStorage` and wins over this value. |
| `base_url` | string | location of the JSON file | Directory used to resolve relative `md_url` values. |
| `sections` | array | required | Sidebar content. `items` is accepted as an alias. |

## Sections

A section is a group with a title and a list of entries:

```json
{ "type": "section", "name": "Guide", "pages": [] }
```

Sections can be nested. `pages` is the conventional key, `items` also works.

## Pages

```json
{ "type": "page", "name": "Getting started", "md_url": "getting-started.md" }
```

- `name` is used in the sidebar, the top bar and the document title.
- `md_url` is fetched at runtime. Relative paths resolve against `base_url`.
- `slug` is optional and auto-generated from `name` for the `#/slug` route.

## Links

```json
{ "type": "link", "name": "GitHub", "url": "https://github.com" }
```

Links are regular anchors and open in a new tab.

## Options

`InitDocs` accepts a second argument:

```js
docmd.InitDocs("docs.json", { target: "#docmd", primary: "#7aa2f7", theme: "light" });
```

Options win over the JSON values. `target` accepts a selector or an element and
defaults to the element with id `docmd`, then `document.body`.

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `target` | string \| element | `#docmd` | Where the documentation is mounted. |
| `primary` | string | JSON `primary` | Accent color. |
| `theme` | string | JSON `theme` | `dark` or `light`. |
| `highlight` | boolean | `true` | Syntax highlight code fences with highlight.js. |
| `copy` | boolean | `true` | Copy button on every code block. |

With the self-initializing script tag the same options are attributes:

```html
<script src="docmd.js" data-docmd="docs.json" data-primary="#7aa2f7" data-theme="light" data-highlight="false" data-copy="false"></script>
```
