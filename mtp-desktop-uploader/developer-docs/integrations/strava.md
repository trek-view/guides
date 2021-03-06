---
description: >-
  Strava is an internet service for tracking human exercise which incorporates
  social network features.
---

# Strava

## **Setup**

To authenticate users to Strava API \(get token for user by granting access\), [you must create a Strava Oauth app here](https://www.strava.com/settings/api).

![](../../../.gitbook/assets/563741c2-e0df-4791-b402-784851a6f21e.png)

[The app requires the `activity:write` and `activity:write` scop](http://developers.strava.com/docs/authentication/#requestingaccess)es.

The call back URL \(redirect URI's\) should be set to `MTPW_DOMAIN`

[See Map the Paths Web docs for more.](../../../mtp-web/developer-docs/api.md#mtpu-greater-than-strava-greater-than-mtpw-greater-than-mtpu)

![Strava API app](../../../.gitbook/assets/a56df6a7-491d-48bd-88de-ad8f828dc5a5.png)

This will give Client ID and secret.

You should set these values in the `.env` file as:

```text
STRAVA_CLIENT_ID=
STRAVA_CLIENT_SECRET=
```

## Workflow

### 1. Validate transport type type

Unlike other integrations, Strava uses GPX files to track users activity.

Strava DOES NOT accept certain transport types. These can be viewed in `transport-methods.json` marked with `"strava_activity": "FALSE"`

```text
  {
    "type": "Car",
    "icon": "fas fa-car",
    "description": "",
    "secondary_parent": "Land",
    "mtp_api": "Car",
    "strava_activity": "FALSE"
  },
```

That is, if Sequence is transport type where `"strava_activity": "FALSE"`, user will not see Strava integration.

### 2. Authenticate

[Strava uses a standard Oauth flow to authenticate users described here.](https://developers.strava.com/docs/authentication/#oauthoverview)

![](../../../.gitbook/assets/explorer-map-the-paths-v2-ui-1-.jpg)

When a user tries to upload gpx to Strava, they will grant the Strava Oauth app access to act on their behalf \(see setup\).

![](../../../.gitbook/assets/5893f5c8-7679-4f4f-af89-dda5c2ad1c40.png)

When they click integrate/authenticate to Strava at integrations step it will open a browser window for user to authorise your app.

If user clicks allow, the browser will redirect the user \(and token generated\) back to the MTP web \(using callback URL -- a dedicated MTPW endpoint for such tokens\).

![](../../../.gitbook/assets/untitled%20%281%29.png)

Token is then automatically passed to MTP Uploader with user automatically redirected to MTP Uploader \(after clicking "open app"\) in browser.

[Strava tokens do expire automatically](https://developers.strava.com/docs/authentication/). As such, MTPDU does not store the token. This authentication is required every single time a user attempts to sync a new Sequence with Strava.

### 3. Upload activity

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

If successful a 201 response is returned with `id` of activity. If error \(4xx/5xx\) attempt to reupload 3 times, then warn user of error and ask them to attempt to try again later.

### 4. Update activity

[Now the app needs to update activity on Strava with activity details \(the transport method used\)](http://developers.strava.com/docs/reference/#api-Activities-updateActivityById) using the Update Activity \(updateActivityById\) endpoint.

This requires 2 values:

* `id` : the activity id obtained during previous step
* `type` : taken from the `strava_activity` value in `transport-methods.json` for sequence transport type.

Requires activity:write. Also requires activity:read\_all in order to update Only Me activities.

Sample request

```text
http PUT "https://www.strava.com/api/v3/activities/{id}" \
    "type" : "value",
    "Authorization: Bearer [[token]]"
```

### 5. MTPW update

Strava information gets synced to Map the Paths web.

The process works in two parts:

**5.1 MTPW token / sequence id**

\*\*\*\*[MTPW authentication must be enabled for this integration for MTPW sync to work](../../../mtp-web/developer-docs/api.md#authorize). As such, app will already have MTPW token when user logged in when opening app.

[The app already has MTPW sequence information following create action of Sequence earlier in the process. ](map-the-paths-web.md)

**5.2 PUT Strava data**

\*\*\*\*[Send Strava info as PUT request to`/api/v1/sequence/import`](../../../mtp-web/developer-docs/api.md#create-sequence)

This can be sent using the PUT `/api/v1/sequence/import/MTP_SEQUENCE_ID` endpoint by including: `strava=true`.

```text
curl --location --request PUT 'https://MTPDOMAIN/api/v1/sequence/import/jjff8djf-jkld87-kls889' \
--data-raw '{
    "strava": TRUE
}'
```

[View the full MTPW API Docs here.](../../../mtp-web/developer-docs/api.md)

