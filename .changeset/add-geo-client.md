---
"@voyantjs/data-sdk": minor
---

Add the `geo` client for the Voyant Data geo API (`/data/geo/v1`): canonical
travel geography (countries, regions, cities, ports, and the waterway family)
over one polymorphic places gazetteer, with multilingual names, coordinates,
hierarchy, and `flows_through` relations, plus a `resolve` endpoint
(provider label/code → canonical place).

Surfaces the raw routes under `client.geo.places.*` and typed resources
`client.geo.{countries,regions,cities,ports,rivers}`; `geo.places.get` returns
the place with its outgoing relations inline (e.g. a river's countries), and a
`placeName(place, lang)` helper reads the multilingual `names` map.
