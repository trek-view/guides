---
description: >-
  Strava is an internet service for tracking human exercise which incorporates
  social network features.
---

# \[PROPOSED\] Strava

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
* `activity:read`: read the user's activity data for activities that are visible to Everyone and Followers, excluding privacy zone data
* `activity:read_all`: the same access as `activity:read`, plus privacy zone data and access to read the user's activities with visibility set to Only You
* `activity:write`: access to create manual activities and uploads, and access to edit any activities that are visible to the app, based on activity read access level

