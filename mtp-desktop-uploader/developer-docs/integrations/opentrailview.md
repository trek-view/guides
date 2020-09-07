# \[PROPOSED\] OpenTrailView

### This document is a WORK IN PROGRESS

#### Overview

1. You use [Sequence Maker](https://github.com/trek-view/sequence-maker) to create photo connections
2. You define the panoramic photos to upload
3. The script validates that they meet the minimum Open Trail View criteria
4. You define the Open Trail View authentication data
5. The script uploads the images toOpen Trail View
6. Open Trail View publishes your images within 72 hours

#### The details

The script first defines the order for upload using [Sequence Maker](https://github.com/trek-view/sequence-maker) in the photo. If no Sequence Maker information exsits, they will be required to first run Sequence Maker on the photos.

The script will then upload photo to the authenticated account.

The script uses OAuth2 \(set up by user\) to communicate with the OTV app.

The OAuth2 URLs are as follows:

* Authorise - [https://opentrailview.org/oauth/auth/authorize](https://opentrailview.org/oauth/auth/authorize)
* Access token - [https://opentrailview.org/oauth/auth/access\_token](https://opentrailview.org/oauth/auth/access_token)

[The OpenTrailView API docs can be found here](https://opentrailview.org/addApp).

This API accepts panoramic Photos to be published on OpenTrailView.

Uploading panoramas is simple using the Upload API endpoint

```text
POST https://opentrailview.org/oauth/api/panorama/upload
```

The upload API takes one POST parameter, 'file', containing the file to be uploaded. Max size = 30MB.

**Responses**

It will return 200, or 400, 401 or 500 depending on the circumstance, together with some JSON describing the success \(and panorama ID\) or otherwise of the upload.

User will be shown appropriate success or error message for each photo.

The script will upload all photos, and will retry 3 times for failures. It will also generate a tmp file that keeps tracks uploads \(and returned photoids\). This is placed in the input folder and will remain after scrip completes \(or fails\). This means you can look into this to find photoids in the future \(if you need to check publish status\) or restart the script from where it left off in case of failures \(the script will check for this file in specified directory prior to execution\).

