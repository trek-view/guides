---
description: Integrate your own app with Map the Paths...
---

# API

{% hint style="info" %}
v1 of the API has been designed to built for use with the MTP Desktop Uploader. If you have feature requests for v2, please submit them on the forum.
{% endhint %}

### Data Types <a id="data-types"></a>

JSON is the default format for all resources. If other formats are avaliable, you can request it by specifying the format in the request's `Accept` header.

### Root Endpoint <a id="root-endpoint"></a>

All resources are accessible through the root endpoint prefixed with current version `v1`. All URLs referenced in the documentation have this endpoint as base path.

```text
https://mtp.trekview.org/api/v1
```

### Client ID <a id="client-id"></a>

Map the Paths uses a client ID to allow access to API v1. You can register an application at your developer registration page. The MTP API expects that the `client_id` parameter is present in all requests.

### OAuth <a id="oauth"></a>

The MT\|P API lets you interact with MTP on the behalf of a user. This is achieved by using OAuth 2.0. MTP supports the implicit and code flow of the OAuth [2.0 specification](http://tools.ietf.org/html/rfc6749).

MTP OAuth tokens do not have any expiration time. The user can at any time revoke the token directly from the settings page.

#### Request Authorization <a id="request-authorization"></a>

{% api-method method="get" host="" path="/authorize?client\_id=<client\_id>&response\_type=<response\_type>" %}
{% api-method-summary %}
Authorize
{% endapi-method-summary %}

{% api-method-description %}
The OAuth authorization endpoint. Your app redirects a user to this endpoint, allowing them to delegated access to their account.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="response\_type" type="string" required=false %}
`implicit` of `token`
{% endapi-method-parameter %}

{% api-method-parameter name="client\_id" type="string" required=false %}
The client ID generated when creating the application on Map the Paths Web. Create a new app here.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Example Request**

```text
curl --location --request GET 'https://mtp.trekview.org/api/v1/authorize?client_id=98fgjsjduf89388&response_type=token'
```







