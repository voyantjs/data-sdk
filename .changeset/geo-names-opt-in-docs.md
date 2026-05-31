---
"@voyantjs/data-sdk": patch
---

geo: clarify in the types + README that the full `names` map is opt-in — geo
reads return only the resolved `name`/`nameLang` by default; pass `names: true`
to also get the full map.
