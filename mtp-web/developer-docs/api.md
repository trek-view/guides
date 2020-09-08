---
description: Integrate your own app with Map the Paths...
---

# API

{% hint style="info" %}
v1 of the API has been designed to built for use with the MTP Desktop Uploader. If you have feature requests for v2, please submit them on the forum.
{% endhint %}

### Outh Applications



### Base URL

{% embed url="https://mtp.trekview.org/api/v1/" %}

### Authorize

{% api-method method="get" host="/authorize?client\_id=<client\_id>&response\_type=token" path="" %}
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
token
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
curl --location --request GET 'https://mtp.trekview.org/api/v1/authorize?client_id=98fgjsjduf89388&client_id%3E&response_type=token'
```

To create an app



