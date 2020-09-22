---
description: Sync MTPDU data to other services
---

# Integrations

### About

[To see how integrations fit into MTPDU read this section of the docs.](../functions.md#20-authenticate-to-integrations)

### Existing integrations

The following pages describe the logic and configuration setting required for each integration:

{% page-ref page="map-the-paths-web.md" %}

{% page-ref page="mapillary.md" %}

{% page-ref page="google-street-view.md" %}

{% page-ref page="strava.md" %}

### Map the Paths Web

Map the Paths Web integration must always be enabled when adding an integration.

### Integrations precedent

Integrations are processed in following order:

1. MTPW \(required\)
2. Mapillary
3. Google Street View
4. Strava

### Building integrations

Integrations are modularised in `ROOT/integrations/`.

Each integration has its own directory using the naming convention `INTEGRATION_NAME`.

Inside each directory is the variables for the module and logo files for UI.

The variables for the integration exist in a file called `module.json`

An example of this file:

```text
{
    "name": "NAME",
    "oauth": {
        "params": { 
            "client_id": "APP_ID",
            "redirect_uri": "REDIRECT_URI"
        },
        "baseUrl": "https://www.app.com/connect?response_type=token"
    }
}
```

The app displayed in the UI is stored in the `/static` directory with the name `module-icon.png`.

