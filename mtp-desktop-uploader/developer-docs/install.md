---
description: Installation instructions from source code...
---

# Install

{% hint style="info" %}
[If you are trying to download and install the latest version of the Map the Paths Desktop Uploader, go here.](http://mtp.trekview.org/uploader)
{% endhint %}

### Environment / build setup

**Pre-requisites**

Requires:

* [Yarn](https://classic.yarnpkg.com/en/)
* [NodeJS](https://nodejs.org/)

#### **Install**

```text
git clone https://github.com/trek-view/mtp-desktop-uploader/
```

```text
cd mtp-desktop-uploader
```

```text
yarn
```

```text
cd app && yarn
```

#### **Environment**

Make a copy of the `.env-sample` file in `/ROOT/` with the name `.env` to add your own environmental variables

```text
cp .env-sample .env
```

**Mapbox**

You must enter a Mapbox Access Token in the new `.env` file for MTPDU to work.

[Create a Mapbox Access Token here](https://account.mapbox.com/). Mapbox is a paid service, [but has a free tier allowing for 50,000 map loads per month](https://www.mapbox.com/pricing/).

Add the Mapbox token in `.env` against this key:

```text
MAPBOX_TOKEN=
```

**Other key=values**

The other key/values in the `.env` file \(e.g. `MTP_WEB_APP_ID=`\) perform non-essential functions described in the App Functions section of this wiki.

They can be left blank without causing any breaking issues, though associated functionality will be limited.

### **Starting Development**

Start the app in the `dev` environment. This starts the renderer process in [**hot-module-replacement**](https://webpack.js.org/guides/hmr-react/) mode and starts a webpack dev server that sends hot updates to the renderer process:

```text
yarn dev
```

### **Packaging for Production**

To package apps for the local platform \(the OS your machine is running\):

```text
yarn package
```

