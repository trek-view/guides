# Mapillary

**Setup**

You need to create a Mapillary App. [Mapillary applications can be created here](https://www.mapillary.com/dashboard/developers).

Most options can be set as desired, but the callback user must be XXXXXXS

You can place your Mapillary application information in the `.env` file once created under the values:

```text
MAPILLARY_APP_ID=
MAPILLARY_SECRET=
```

**Authentication**

The Mapillary API lets you interact with Mapillary on the behalf of a user. [This is achieved by using OAuth 2.0](https://www.mapillary.com/developer/api-documentation/#oauth).

**Upload**

Mapillary is based on a very similar structure to Map the Paths with [Sequences](https://www.mapillary.com/developer/api-documentation/#sequences) and [Images](https://www.mapillary.com/developer/api-documentation/#images).

Sequences on Mapillary do not contain as much data \(e.g. names and descriptions\) as set on MTPU.

Mapillary accepts both flat and equirectangular images.

[There are 4 steps to upload imagery to Mapillary](https://www.mapillary.com/developer/api-documentation/#uploading-imagery):

1. Prepare the imagery for uploading \(this is already done by the app in ImageDescription of images\) on MTP Desktop
2. User authenticates to Mapillary
3. Create an upload session on Mapillary
4. Upload the imagery to the upload session on Mapillary
5. Publish the upload session on Mapillary

[For the purpose of testing, you can call the API with query parameter "dry run" to tell the service not publish the session for real \(note that you still won't reach the session after the call\). The session will fail after a few weeks](https://www.mapillary.com/developer/api-documentation/#publish-an-upload-session).

**Store**

Mapillary provides ongoing status of an [upload session](https://www.mapillary.com/developer/api-documentation/#the-open-upload-session-object), [including failures](https://www.mapillary.com/developer/api-documentation/#the-failed-upload-session-object).

When an image and sequence is created, the Map the Paths Desktop Uploaded stores the Mapillary info for it as shown in linked JSON examples for:

* [Sequences](https://www.mapillary.com/developer/api-documentation/#the-sequence-object)
* [Images](https://www.mapillary.com/developer/api-documentation/#the-image-object)

**MTPW sync**

![alt-text](https://raw.githubusercontent.com/wiki/trek-view/mtp-desktop-uploader/images/mtpu-mapillary-mtpw.png)

Mapillary sequence information gets synced to Map the Paths Web. When sequence ID posted, the sequence is imported to the users MTPW acccount \(the user account that authenticated to MTPU\).

[This is an automated version of the manual import sequence function in the MTPW UI. I strongly recommend testing how the manual process works here](https://mtp.trekview.org/sequence/import-sequence-list).

The process uses the `/api/v1/sequence/import` on MTPW and requires the following information.

**Mapillary information sent**

* `sequence_key`
* `username`
* `user_key`
* `token`

**Map the Paths information sent**

* `name`
* `description`
* `transport_type`
* `tags`

**An example**

```text
curl --location --request POST 'https://mtp.trekview.org/api/v1/sequence/import' \
--form 'mapillary_data={"sequence_key": "cHBf9e8n0pG8O0ZVQHGFBQ", "username": "trekviewhq", "user_key": "AGfe-07BEJX0-kxpu9asf", "token": "8374jdhlad874"}' \
--form 'name="My first sequence"' \
--form 'description="Just testing the API"' \
--form 'transport_type="Land-Bike"' \
--form 'tag=tag1,tag2,tag3,tag4'
```

[View the MTPW API Docs here](https://documenter.getpostman.com/view/11425903/TVCfVnZ2)

