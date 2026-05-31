---
"@voyantjs/data-sdk": patch
---

geo: make `CanonicalPlace.names` optional. The full `names` map is omitted from
geo responses by default (you opt in with `names: true`); typing it as required
let `place.names.en` compile while being `undefined` at runtime. Guard before
indexing, or use the resolved `name` field / `placeName` helper.
