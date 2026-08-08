Just somewhere to try linking to templates for ROCSS.

## Template folder conventions

Each top-level folder in this repo is one selectable template. Two shapes
are supported:

### Single-page (flat)

One folder, no subfolders: a `config.json`, a `style.css`, and one
`template.html`, referenced by bare filename from `config.json`
(`"root": {"template": "template.html"}`, `"style": "style.css"`). See
`dyirbal/` for a working example. Use this when one root template covers
every page (`config.json`'s `multipage` is `false` or omitted).

### Multipage (bundle)

A folder containing a `templates/` subfolder with one `.html` file per
role, referenced from `config.json` by path relative to the *template
folder* (not the subfolder):

```
<folder>/
  config.json
  templates/
    root-template.html
    <other-role>-template.html
```

```json
{
  "multipage": true,
  "root": { "template": "templates/root-template.html" },
  "types": {
    "SomeType": { "template": "templates/some-type-template.html" }
  }
}
```

`config.json`'s `types` map lets different RO-Crate entity `@type`s render
through different templates (e.g. one for a Collection page, another for a
Document page), instead of a single template handling every page. See
`structured-docs/` for a working example, ported from a project-specific
multipage site originally built directly against an RO-Crate.

## Per-site keys

Two `config.json` keys are deliberately absent from the templates in this repo,
because their values only make sense for one particular crate. Add them to your
own copy of a config when you need them:

- **`homePageId`** — the `@id` of an entity to render as the landing page
  instead of the root entity. The `structured-docs` root template resolves it
  and falls back to the root view when it is missing or does not match an
  entity in the crate — so a stale value fails *silently*, showing the root
  page with no error. Check it against a real `@id` in your
  `ro-crate-metadata.json`.
- **`domain`** — the bare hostname the site will be published under. It is used
  to build absolute `og:url` and `og:image` values for social-media preview
  cards; without it those tags are omitted.

```json
{
  "homePageId": "#MyCollection-HOME",
  "domain": "https://example.org/my-site"
}
```

## hideInTable

`hideInTable` is supported on `navigationByType` column entries in `config.json`.

- Set `hideInTable: true` to hide that property from table columns for that type.
- Keep `addFacet: true` if you still want it available as a filter.

## Tabular Column Visibility (navigationByType)

In `config.json`, each column entry under `navigationByType` can include:

- `hideInTable: true` to hide that column from the visible table header/cells.
- `addFacet: true` to keep using that same property as a filter facet.

This allows a property to be filter-only for specific types.

Example (`MediaObject` table):

```json
{
	"uri": "http://purl.org/dc/terms#format",
	"label": "Format",
	"addFacet": true,
	"facetLabel": "format",
	"hideInTable": true
}
```

Notes:

- `hideInTable` is per navigation entry, so the same property can be visible in one type table and hidden in another.
- This does not remove filter support; with `addFacet: true`, the facet still appears (for example, `Filter by format`).
