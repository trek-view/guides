# \[PROPOSED\] Google Street View

### This document is a WORK IN PROGRESS

#### 20.3 Google Street View

**Setup**

You will need a Google Cloud project to access the API's. Login to the [GCP Console](https://console.developers.google.com/).

The project name can be anything you want. It will only be visible to you in the GCP Console.

This app requires the following Google API's to work:

* [Google Geocoding API](https://console.cloud.google.com/google/maps-apis/apis/geocoding-backend.googleapis.com) \(to lookup address info and place information against each photo\). [This is a paid for service](https://developers.google.com/maps/documentation/geocoding/usage-and-billing).
* [Street View Publish API](https://console.cloud.google.com/apis/library/streetviewpublish.googleapis.com) \(used to send photos to Street View\)

To enable these services, click each of the links above \(making sure the menu bar at the top shows the project you just created\) and select enable.

If this is your first time creating a project you might see the message:

> "To create an OAuth client ID, you must first set a product name on the consent screen."

Click the "Configure consent screen" button to do this.

The only field you need to fill in is "Application name". This name will be shown when you authenticate to allow the script to use your Google account to publish to Street View.

Once you have set the required consent information, select "API & Services" &gt; "Credentials".

![GCP create credentials](/images/gcp-create-credentials.png)

Now select "Create credentials" &gt; "OAuth client ID"

![GCP create OAuth client ID](/images/gcp-create-oauth-client-id.png)

Select Application type as "XXXXX".

Enter a name for the credentials. This is helpful for tracking who these credentials are for.

![GCP OAuth credentials](/images/gcp-oauth-credentials.png)

If everything is successful, Google will generate a client ID and client secret. Make a note of these.

You can place your Google application information in the `.env` file once created.

Note we split each API allowing the possibility to use different Google API keys for each service. If you want to use the same project/keys for both API's you can use identical `ID` and `SECRET` values for each key. You also have the option to use different keys for different API's. This can be useful in cases where you want to split the paid \(Geocoding\) and free \(Street View Publish\) API's between projects for billing purposes.

```text
GOOGLE_MAPS_STREETVIEW_PUBLISH_ID=
GOOGLE_MAPS_STREETVIEW_PUBLISH_SECRET=
GOOGLE_MAPS_GEOCODING_ID=
GOOGLE_MAPS_GEOCODING_SECRET=
```

**Upload**

Google only has a concept of images.

Google only accepts equirectangular images.

[We've written an introduction to the Street View Publish API here](https://www.trekview.org/blog/2020/street-view-publish-api-quick-start-guide/). It is a useful guide in quickly understanding the fields that can be utilised with the API.

[The photo.pose resource takes the following values](https://developers.google.com/streetview/publish/reference/rest/v1/photo#pose).

* "captureTime" \(required\) &gt; GPSDateTime
* "latitude" \(required\) &gt; GPSLatitude
* "longitude" \(required\) &gt; GPSLongitude
* "altitude" \(required\) &gt; GPSAltitude
* "heading" \(optional\) &gt; heading \(for connection with positive connection.distance\_mtrs\)
* "pitch" \(optional\) &gt; pitch \(for connection with positive connection.distance\_mtrs\)

After user has authenticated \(via OAuth with Google\) they are required to enter a place for their images.

[Users must also set a place ID](https://developers.google.com/streetview/publish/reference/rest/v1/photo#place). User searches an address, [and is matched to Google Maps place using the geocoding API](https://console.cloud.google.com/google/maps-apis/apis/geocoding-backend.googleapis.com). The single place value is assigned to all images in sequence.

_A note on Connections / Blue Lines_

It’s important to note, Google does some server side processing of Street View images too.

[Google recommends when taking 360 Street View images](https://support.google.com/maps/answer/7012050?hl=en&ref_topic=627560).

> Space the photos about two small steps apart \(1 m / 3 ft\) when indoors and five steps apart \(3 m / 10 ft\) when outdoors.

This video from the Street View Conference in 2017 also [references the 3m interval](https://www.youtube.com/watch?v=EW8YKwuFGkc).

[According to this Stack Overflow answer](https://stackoverflow.com/questions/54237231/how-to-create-a-path-on-street-view):

> You need to have &gt; 50 panoramas with a distance &lt; 5m between two connected panoramas. After some days \(weeks?\) Google will convert them to a blue line in a separate processing step.

It’s safe to assume in some cases Google servers might automatically connect your images into a blue line even if a connection target is not defined, [as addressed here](https://support.google.com/contributionpolicy/answer/7411351):

> When multiple 360 photos are published to one area, connections between them may be automatically generated. Whether your connections were created manually or automatically, we may adjust, remove, or create new connections — and adjust the position and orientation of your 360 photos — to ensure a realistic, connected viewing experience

In summary, you're photos can be connected to other photos on the Street View platforms, in addition to the connections defined in the script.

**Store**

[Google provides ongoing transfer status for each image uploaded](https://developers.google.com/streetview/publish/reference/rest/v1/photo#transferstatus).

When an image is created, the Map the Paths Desktop Uploader stores the Google Street View info for it as shown in linked JSON example:

```text
{
  "photoId": {
    object (PhotoId)
  },
  "uploadReference": {
    object (UploadRef)
  },
  "downloadUrl": string,
  "thumbnailUrl": string,
  "shareLink": string,
  "pose": {
    object (Pose)
  },
  "connections": [
    {
      object (Connection)
    }
  ],
  "captureTime": string,
  "places": [
    {
      object (Place)
    }
  ],
  "viewCount": string,
  "transferStatus": enum (TransferStatus),
  "mapsPublishStatus": enum (MapsPublishStatus)
}
```

