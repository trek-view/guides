---
description: >-
  Guidebooks are collections of any Mapillary photos with annotations to provide
  more information.
---

# Guidebooks

### Overview

* Guidebook belongs to 1 user
* Guidebook has &gt;= 1 photos
  * Photo does not need to belong to imported MTP sequence. Can be any Mapillary image
* Guidebook has like count
  * Any user \(except owner can mark TRUE/FALSE\)
* Guidebook has 
  * A title
  * A description \(optional\)
  * A category
  * &gt;=0 descriptive tags

### Guidebook creation workflow

#### Adding scene \(photo\)

Guidebooks work with any photo uploaded \(and public\) on Mapillary.

When user brings map to low zoom level we query the [Mapillary Image Search API ](https://www.mapillary.com/developer/api-documentation/#search-images)using `bbox` \(bounding box\) parameter to return sequences \(and photo postitions\) in that area,.

Example request:

```text
curl "https://a.mapillary.com/v3/images?bbox=[min_longitude,min_latitude,max_longitude,max_latitude]client_id=<YOUR_CLIENT_ID>"
```

Example response:

```text
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "ca": 0,
        "camera_make": "",
        "camera_model": "",
        "captured_at": "1970-01-01T00:00:03.000Z",
        "key": "QKCxMqlOmNrHUoRTSrKBlg",
        "pano": true,
        "sequence_key": "KrZrFFEzBszRJTaBvK3aqw",
        "user_key": "T8XscpSs_3_W673i7WFQug",
        "username": "underhill",
        "organization_key": "GWPwbGxhu82M5HiAeeuduH",
        "private": false
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          -135.6759679,
          63.8655195
        ]
      }
    },
    {
      "type": "Feature",
      "properties": {
        "ca": 328.25359712230215,
        "camera_make": "LG Electronics",
        "camera_model": "LG-R105",
        "captured_at": "2020-07-18T14:14:36.555Z",
        "key": "cCkQ6Fd9Nigw5EYV8qWEIw",
        "pano": true,
        "sequence_key": "jxXeLZBG0oIMkEXnBZBATA",
        "user_key": "3gJd_jiCTawMEck87fRj_Q",
        "username": "jmmapiranger"
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          139.9049049,
          37.5309416
        ]
      }
    }
  ]
}
```

Using the returned `features.geometry.coordinates` we plot locations on the map.

When a point is clicked, we can is the `features.property.key` to load the photo in [Mapillary JS](https://github.com/mapillary/mapillary-js).

#### Adding point of interest \(POI\)





