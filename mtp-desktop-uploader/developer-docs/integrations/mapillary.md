---
description: >-
  Mapillary is the street-level imagery platform that scales and automates
  mapping using collaboration, cameras, and computer vision.
---

# Mapillary

### **Setup**

The Mapillary API lets you interact with Mapillary on the behalf of a user. [This is achieved by using OAuth 2.0](https://www.mapillary.com/developer/api-documentation/#oauth).

You need to create a Mapillary App. [Mapillary applications can be created here](https://www.mapillary.com/dashboard/developers).

![Mapillary register app](../../../.gitbook/assets/mapillary-register-app.png)

Most options can be set as desired, but the callback user must match that specified in the `.env` file.

Enable all scopes \("allow this application to"\).

You can place your Mapillary application information in the `.env` file once created under the values:

```text
MAPILLARY_APP_ID=
MAPILLARY_SECRET=
MAPILLARY_REDIRECT_URI=
```

### **Upload images**

Mapillary is based on a very similar structure to Map the Paths with [Sequences](https://www.mapillary.com/developer/api-documentation/#sequences) and [Images](https://www.mapillary.com/developer/api-documentation/#images).

Sequences on Mapillary do not contain as much data \(e.g. names and descriptions\) as set on MTPDU.

Mapillary accepts the following image projections:

* flat
* equirectangular

Mapillary DOES NOT accept the following transport types

* Air
  * All child elements

[There are 4 steps to upload imagery to Mapillary](https://www.mapillary.com/developer/api-documentation/#uploading-imagery):

1. Prepare the imagery for uploading \(this is already done by the app in `ImageDescription` of images\) on MTPDU \([see here](../functions.md#21-2-imagedescription-json-object)\)
2. User authenticates to Mapillary
3. Create an upload session on Mapillary
4. Upload the imagery to the upload session on Mapillary
   1. The app checks the status of the open upload session using the [open upload session endpoint](https://www.mapillary.com/developer/api-documentation/#the-open-upload-session-object).
5. Publish the upload session on Mapillary \(by closing the upload session\)

[For the purpose of testing, you can call the API with query parameter "dry run" to tell the service not publish the session for real \(note that you still won't reach the session after the call\). The session will fail after a few weeks](https://www.mapillary.com/developer/api-documentation/#publish-an-upload-session).

### **Get Mapillary Sequence ID**

Unfortunately Mapillary does not provide the Sequence\_Key for an upload session in a response.

Therefore we need to do a bit of filtering with the Mapillary Sequences endpoint to get this info:

#### 1. Get upload user

After the user obtains the oAuth token, the app makes an API call to [`https://a.mapillary.com/v3/me`](https://a.mapillary.com/v3/me) with the users token to get the caller's user\_key.

#### 2. Request user sequences

It is not possible to query the Mapillary API Search Sequence endpoint to find the Sequence Key of the Sequence uploaded:

* user\_key: obtained in step one
* start\_time: time of first photo in UTC uploaded in sequence \(`MAPCaptureTime`\)
* end\_time: time of last photo in UTC uploaded in sequence \(`MAPCaptureTime`\)

We can now form a request: 

```text
curl "https://a.mapillary.com/v3/sequences?userkeys=USER_KEY&start_time=TIME&end_time=TIMEclient_id=<YOUR_CLIENT_ID>"
```

This will return a response like:

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

Where `features.key` is the value of the Sequence Key we need to store.

{% hint style="info" %}
Design decision: theoretically it is possible a user has two sequences in different places with start and end times in the specified range. However, it was decided because such a scenario is VERY unlikely, we accepted the risk of such collisions.
{% endhint %}

Mapillary provides ongoing status of an [upload session](https://www.mapillary.com/developer/api-documentation/#the-open-upload-session-object), [including failures](https://www.mapillary.com/developer/api-documentation/#the-failed-upload-session-object).

When an image and sequence is created, the MTPDU stores the Mapillary Sequence Key.

### **MTPW sync**

![MTPDU, MTPW and Mapillary sycn](../../../.gitbook/assets/explorer-v2-diagrams-3-%20%281%29.jpg)

Mapillary sequence ID information gets synced to Map the Paths Web. 

[This is an automated version of the manual import sequence function in the MTPW UI. I strongly recommend testing how the manual process works here](https://mtp.trekview.org/sequence/import-sequence-list).

The process works in three parts:

1. Get MTPW token \(authentication must be enabled for this integration for MTPW sync to work. As such, app will already have MTPW token when user logged in when opening app\)
2. Send Mapillary OAuth token to MTPW. In order for MTPW to communicate with Mapillary, it needs a copy of the Mapillary OAuth token. This can be sent using the `api/v1/mapillary/token/verify` endpoint using users `mapillary_token` value.
3. Now the required MTPDU and Mapillary sequence data can be submitted to MTPW. This can be sent using the `/api/v1/sequence/import` endpoint by including: `sequence_key` \(Mapillary\), `name` \(MTPDU\), `description` \(MTPDU\), `transport_type` \(MTPDU\), `tags` \(MTPDU\)

[View the full MTPW API Docs here.](../../../mtp-web/developer-docs/api.md)

[You can see the way the Sequence is then created on Map the Paths web here.](../../../mtp-web/developer-docs/functions/sequences.md)

