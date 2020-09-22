---
description: >-
  Strava is an internet service for tracking human exercise which incorporates
  social network features.
---

# Strava

### **Setup**

To authenticate users to Strava API \(get token for user by granting access\), [you must create a Strava Oauth app here](https://www.strava.com/settings/api).



![Strava API app](../../../.gitbook/assets/a56df6a7-491d-48bd-88de-ad8f828dc5a5.png)

This will give Client ID and secret.

You should set these values in the `.env` file as:

```text
STRAVA_CLIENT_ID=
STRAVA_CLIENT_SECRET=
```



[http://developers.strava.com/docs/authentication/\#requestingaccess](http://developers.strava.com/docs/authentication/#requestingaccess)

* `read`: read public segments, public routes, public profile data, public posts, public events, club feeds, and leaderboards
* `read_all`:read private routes, private segments, and private events for the user
* `profile:read_all`: read all profile information even if the user has set their profile visibility to Followers or Only You
* `profile:write`: update the user's weight and Functional Threshold Power \(FTP\), and access to star or unstar segments on their behalf



### Workflow

#### 1. GPX file

Unlike other integrations, Strava uses GPX files to track users activity

#### 2. Authenticate

[Strava uses a standard Oauth flow to authenticate users described here.](https://developers.strava.com/docs/authentication/#oauthoverview)

#### 3. Upload activity

[You can create an activity using the createUpload Strava endpoint](http://developers.strava.com/docs/reference/#api-Uploads-createUpload).

Requires `activity:write` scope.

Sample request

```text
http POST "https://www.strava.com/api/v3/uploads" \
    file@/path/to/file \
    name='value' \
    description='value' \
    data_type='value' \
    external_id='value' \
    "Authorization: Bearer [[token]]"
```

These map to MTP Uploader values like so:

* `file`=GPX file path \(in sequence directory\)
* `name`=sequence name
* `description`=sequence description
* `data_type`=gpx
* `external_id`=map the paths web uuid

If successful a 201 response is returned. If error \(4xx/5xx\) attempt to reupload 3 times, then warn user of error and ask them to attempt to try again later.

#### 4. MTPW update

Strava information gets synced to Map the Paths web.

The process works in two parts:

**6.1 MTPW token / sequence id**

\*\*\*\*[MTPW authentication must be enabled for this integration for MTPW sync to work](../../../mtp-web/developer-docs/api.md#authorize). As such, app will already have MTPW token when user logged in when opening app.

[The app already has MTPW sequence information following create action of Sequence earlier in the process. ](map-the-paths-web.md)

**6.2 PUT Strava data**

\*\*\*\*[Send Strava info as PUT request to`/api/v1/sequence/import`](../../../mtp-web/developer-docs/api.md#create-sequence)

This can be sent using the PUT `/api/v1/sequence/import/MTP_SEQUENCE_ID` endpoint by including: `strava=true`.

```text
curl --location --request PUT 'https://mtp.trekview.org/api/v1/sequence/import/jjff8djf-jkld87-kls889' \
--data-raw '{
    "strava": TRUE
}'
```

[View the full MTPW API Docs here.](../../../mtp-web/developer-docs/api.md)

