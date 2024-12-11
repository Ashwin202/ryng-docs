---
description: >-
  Cancel Calls API can be used to cancel uploaded fresh calls and scheduled
  calls that are yet to be attempted.
---

# Cancel Calls API

**URL**

`https://`_`<subdomain>`_`.ryng.in/rest-api/v1/progressive/call/`_`<callId>`_`/cancel`

* Replace _`<subdomain>`_ with your account subdomain.
* Replace `callId`  with the call ID that you want to cancel.

**Method**

**`POST`**

**Headers**

| Key            | Description                                                                                                                                                                                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x-access-token | The access token is provisioned for you by Ryng. If you do not have this already, please reach out to your Ryng project champion. Alternatively, you can raise a request on our [support portal](https://cod.atlassian.net/servicedesk/customer/portal/2). |

## API Responses

***

## Cancel API

<mark style="color:green;">`POST`</mark> `https://tenant.ryng.in/rest-api/v1/progressive/call/:callId/cancel`

#### Headers

| Name                                           | Type   | Description |
| ---------------------------------------------- | ------ | ----------- |
| x-access-key<mark style="color:red;">\*</mark> | String |             |

{% tabs %}
{% tab title="200: OK Success" %}
```json
{
        "error": false,
        "message": "Call Cancelled",
        "data": {}
}
```
{% endtab %}

{% tab title="403: Forbidden Authentication Error" %}
```json
{
    "error": true,
    "message": "Unauthorized Token"
}
```
{% endtab %}

{% tab title="500: Internal Server Error Failed attempting to cancel a Call ID that doesn’t exist" %}
```json
{
        "error": true,
        "message": "Call ID does not exist"
}
```
{% endtab %}
{% endtabs %}

