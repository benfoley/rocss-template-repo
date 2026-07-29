Just somewhere to try linking to templates for ROCSS.

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
