---
"@voyantjs/data-sdk": patch
---

geo: make `CanonicalPlace.names` optional. A geo call with `names: false` omits
the map from the response, so the field is not always present — typing it as
required let `place.names.en` compile while being `undefined` at runtime. Guard
before indexing, or use the resolved `name` field / `placeName` helper.
