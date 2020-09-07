# \[PROPOSED\] Facebook

### This document is a WORK IN PROGRESS

#### Overview

1. User authenticates
2. User define the text of the post to accompany the post
3. The script starts the upload of content to Facebook until upload complete and images published

#### The details

Authenticating to Facebook required a Facebook app to authenticate. In order to use this script, you'll need to first create your own Facebook app to generate an app id and app secret.

[https://stackoverflow.com/questions/21978728/obtaining-a-facebook-auth-token-for-a-command-line-desktop-application](https://stackoverflow.com/questions/21978728/obtaining-a-facebook-auth-token-for-a-command-line-desktop-application)

[You can create a Facebook app here](https://developers.facebook.com/apps). You will need to enable the product "Facebook login".

[360 Photos must fulfill the following requirements for Facebook to process them properly](https://facebook360.fb.com/editing-360-photos-injecting-metadata/) \(these are validated by the script\):

* The photo must have a 2:1 aspect ratio
* The Exif XMP tag, "ProjectionType=equirectangular"
* Photos should be less than 30,000 pixels in any dimension, and less than 135,000,000 pixels in total size
* File sizes could be as big as 45 MB \(JPEG\) or 60 MB \(PNG\). We recommend using JPEG for 360 photos and keeping the file size less than 20-30 MB.

The [Facebook API uses a similar standard to Google for 360 images](https://developers.facebook.com/docs/graph-api/reference/photo/#fields). [Google standard for reference](https://developers.google.com/streetview/spherical-metadata).

For video files, the script takes advantage of the [Facebook API's Resumable Uploading function](https://developers.facebook.com/docs/graph-api/video-uploads/#resumable). Videos are published to graph-video.facebook.com instead of the regular Graph API URL \(graph.facebook.com\).

