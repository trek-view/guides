# User Guide

### Install

The Map the Paths Uploader works on both Mac and Windows machines.

Please go to Map the Paths to download the latest version: [http://mtp.trekview.org/uploader](http://mtp.trekview.org/uploader)

### Login

You must have a Map the Paths account to use the Map the Paths Uploader 

Find out more and create an account at: [https://mtp.trekview.org](https://mtp.trekview.org)

### Create Sequence

#### Required information

You need to add some basic information to create a sequence. If you choose to upload your sequence to an integration \(see Integrations\) some of this data will be shared with the selected services.

You are required to select a camera type, if your camera is not listed it means we have not tested the files it produces with the software. In such cases, select "Other camera". In most cases you will not experience problems, however, we cannot guarantee this. [Please post on the support forum if you would like us to include support for a new camera.](https://campfire.trekview.org/c/support/8)

The software accepts both image \(`.jpg`\) and video files \(`.mp4`\)

#### Advanced settings

You can make the following changes under advance settings:

* Attach a GPS track
  * This is a required step if not GPS exists in your images. However, if you have a more accurate GPS track log than the positions recorded in your images you can attach the more accurate GPS track and overwrite camera reported positions.
* Modify spacing between photos
  * Use this if you have lots of closely spaces images \(e.g. when you continued to record standing still\)
* Modify photo outliers
  * If some points are far off track you can either 1\) remove them automatically, 2\) get the software to estimate the actual position of corrupted photos
* Modify azimuth \(heading\)
  * If the photos are not facing the right direction \(shown using the icons on map\) you can change the offset of the heading. This is useful when mis-calibrated sensors report wrong headings.
* Add copyright information
  * Add copyright data to the fields of the processed images for correct attribution
* Add a nadir
  * Upload and add a custom logo as a nadir in your imagery. This is only available for 360 photos.

### Integrations

#### Map the Paths

Map the Paths allows you to share your Sequences with the world.

Find out more and create an account at: [https://mtp.trekview.org](https://mtp.trekview.org)

{% hint style="info" %}
In order to use an integration, you must sync the data with Map the Paths and Mapillary.
{% endhint %}

#### Mapillary

Mapillary is the street-level imagery platform.

Find out more and create an account at: [https://www.mapillary.com/app/](https://www.mapillary.com/app/)

#### Google Street View

Google Street View is a technology featured in Google Maps and Google Earth that provides interactive panoramas from positions along many streets in the world.

Find out more and create a Google account at: [https://www.google.com/maps](https://www.google.com/maps)

**A note on Connections / Blue Lines**

It’s important to note, Google does some server side processing of Street View images too.

[Google recommends when taking 360 Street View images](https://support.google.com/maps/answer/7012050?hl=en&ref_topic=627560).

> Space the photos about two small steps apart \(1 m / 3 ft\) when indoors and five steps apart \(3 m / 10 ft\) when outdoors.

This video from the Street View Conference in 2017 also [references the 3m interval](https://www.youtube.com/watch?v=EW8YKwuFGkc).

[According to this Stack Overflow answer](https://stackoverflow.com/questions/54237231/how-to-create-a-path-on-street-view):

> You need to have &gt; 50 panoramas with a distance &lt; 5m between two connected panoramas. After some days \(weeks?\) Google will convert them to a blue line in a separate processing step.

It’s safe to assume in some cases Google servers might automatically connect your images into a blue line even if a connection target is not defined, [as addressed here](https://support.google.com/contributionpolicy/answer/7411351):

> When multiple 360 photos are published to one area, connections between them may be automatically generated. Whether your connections were created manually or automatically, we may adjust, remove, or create new connections — and adjust the position and orientation of your 360 photos — to ensure a realistic, connected viewing experience

In summary, you're photos can be connected to other photos on the Street View platforms, in addition to the connections defined in the script.

**A note on Local Guides**

All images uploaded through Map the Paths Uploader will contribute to your Local Guides points score.

Points cannot be assigned retrospectively \(we think!\) so please make sure to subscribe to the Local Guides program before uploading images here: [https://maps.google.com/localguides](https://maps.google.com/localguides)

#### Strava

Strava is an internet service for tracking human exercise which incorporates social network features. It is mostly used for cycling and running using GPS data.

Find out more and create an account at: [https://www.strava.com/](https://www.strava.com/)

### Support

Having problems? [Ask a question around the Campfire \(our community forum\)](https://campfire.trekview.org/c/support/8).

