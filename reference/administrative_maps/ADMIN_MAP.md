# Administrative Boundary Data

Source: [Natural Earth](https://www.naturalearthdata.com/), 1:10m Cultural
Vectors, public domain — no attribution legally required, credited here as
good practice.

Used to build the geocell scheme: countries (admin-0) define the outer
merge boundary, states/provinces (admin-1) are the finest unit merged
together until each geocell has enough training points (see the notebook's
"Geocell creation" section).

## Files expected here

- `ne_10m_admin_0_countries.geojson` — country boundaries. Country name
  column used by the pipeline: `ADMIN`.
- `ne_10m_admin_1_states_provinces.geojson` — state/province boundaries.
  Name column used by the pipeline: `name`.

## Download

- Official pages (shapefile format):
  - https://www.naturalearthdata.com/downloads/10m-cultural-vectors/10m-admin-0-countries/
  - https://www.naturalearthdata.com/downloads/10m-cultural-vectors/10m-admin-1-states-provinces/
  - Convert with: `ogr2ogr -f GeoJSON output.geojson input.shp`
- Pre-converted GeoJSON (Natural Earth's own GitHub mirror):
  - `https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_10m_admin_0_countries.geojson`
  - `https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_10m_admin_1_states_provinces.geojson`

## Version used for this repo's reference/ scheme

Natural Earth versions each layer independently, so the two files here can
legitimately (and did, in this repo) land on different version numbers —
that's not a mismatch, just each layer's own most recent correction.

- `ne_10m_admin_0_countries.geojson` — version 5.1.1
- `ne_10m_admin_1_states_provinces.geojson` — version 5.1.0
