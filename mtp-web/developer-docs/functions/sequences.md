---
description: >-
  Sequences are collections of photos continuously captured by a user at a given
  location.
---

# Sequences

### Overview

* Sequence belongs to 1 user
* Sequence has &gt;= 1 photos
* Sequence has like count
  * Any user \(except owner can mark TRUE/FALSE\)
* Sequence can belong to 1 or more Tour owned by Sequence owner
* Sequence object has metadata:
  * A title
  * A description \(optional\)
  * A transport type \(parent / child\)
  * &gt;=0 descriptive tags
  * Camera make
  * Captured date
  * Created date \(uploaded to Mapillary\)
  * [Mapillary Sequence key](https://www.mapillary.com/developer/api-documentation/#sequences)
  * Coordinate properties
    * Mapillary Photo keys
  * Is panorama

### Preliminary Sequence creation workflow

Users can add Sequences to Map the Path in one of two ways:

#### 1. Using Map the Paths Desktop Uploader

[You can read how the Map the Paths Desktop Uploader posts a Sequence to the MTP Web API here.](../../../mtp-desktop-uploader/developer-docs/integrations/mapillary.md)

####  **2. By directly importing from Mapillary in MTP Web**

Users can choose 

1. User authenticates to the Mapillary and grants the client\_id access to their account \(obtained by creating app in Mapillary\)
2. App stores users Mapillary token to authenticate to Mapillary using their account
3. App queries Mapillary sequences belonging to user
4. User selects Mapillary sequences that belong to them and they want to import

### Sequence creation publish workflow

![MTP Sequence Create](../../../.gitbook/assets/explorer-v2-diagrams-2-.jpg)

#### \*\*\*\*[Map the Paths imports all Mapillary Sequence data for sequence\_key user has selected for import using the /v3/sequences endpoint.](https://www.mapillary.com/developer/api-documentation/#sequences)

```text
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "camera_make": "Apple",
        "captured_at": "2016-03-14T13:44:53.860Z",
        "coordinateProperties": {
          "cas": [
            323.032,
            320.892,
            333.622,
            329.948
          ],
          "image_keys": [
            "LwrHXqFRN_pszCopTKHF_Q",
            "Aufjv2hdCKwg9LySWWVSwg",
            "QEVZ1tp-PmrwtqhSwdW9fQ",
            "G_SIwxNcioYeutZuA8Rurw"
          ]
        },
        "created_at": "2016-03-17T10:47:53.106Z",
        "key": "LMlIPUNhaj24h_q9v4ArNw",
        "pano": false,
        "user_key": "AGfe-07BEJX0-kxpu9J3rA",
        "username": "pierregeo"
      },
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [
            16.432958,
            7.246497
          ],
          [
            16.432955,
            7.246567
          ],
          [
            16.432971,
            7.248372
          ],
          [
            16.432976,
            7.249027
          ]
        ]
      }
    }
  ]
}
```

\*\*\*\*



