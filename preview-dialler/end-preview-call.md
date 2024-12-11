# End Preview Call

**URL**

`https://`_`<subdomain>`_`.ryng.in/rest-api/v1/preview/call/`_`:<callID>`_

* Replace _`<subdomain>`_ with your account subdomain.

**Method**

`PATCH`

**Headers**

| Key            | Description                                                                                                                                                                                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x-access-token | The access token is provisioned for you by Ryng. If you do not have this already, please reach out to your Ryng project champion. Alternatively, you can raise a request on our [support portal](https://cod.atlassian.net/servicedesk/customer/portal/2). |

**Params**

For this API, params are either added by `query` params or can be given inside the request `body`&#x20;

Type:  _query params/body_

<table><thead><tr><th width="204">Key</th><th width="81">Type</th><th width="348">Description</th><th>Required?</th></tr></thead><tbody><tr><td>logID</td><td>int</td><td>Agent to be assigned for the call.</td><td>Yes</td></tr><tr><td>dispositionName</td><td>text</td><td>Disposition to be added.</td><td>Yes</td></tr><tr><td>notes</td><td>text</td><td>Notes related to that call.</td><td>Yes</td></tr></tbody></table>

## Example Body

```json
{
    "logID": 87278,
    "dispositionName": "Interested",
    "notes": "The customer was interested in the pitch and asked for a follow-up"
}
```

## API Responses

## Append Campaign API

<mark style="color:green;">`POST`</mark> `https://tenant.ryng.in/rest-api/v1/preview/call`

#### Headers

| Name                                             | Type | Description            |
| ------------------------------------------------ | ---- | ---------------------- |
| x-access-token<mark style="color:red;">\*</mark> |      | Provided by Ryng Team  |

{% tabs %}
{% tab title="200: OK Success Response" %}
```json
{
    "error": false,
    "message": "Call disposed and ended.",
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

{% tab title="500: Internal Server Error Failed" %}
```json
{
    "error": true,
    "message": "Failed to dispose of call."
}
```
{% endtab %}
{% endtabs %}
