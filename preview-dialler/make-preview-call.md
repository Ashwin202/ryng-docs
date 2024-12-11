# Make Preview Call

**URL**

`https://`_`<subdomain>`_`.ryng.in/rest-api/v1/preview/call`

* Replace _`<subdomain>`_ with your account subdomain.

**Method**

**`POST`**

**Headers**

| Key            | Description                                                                                                                                                                                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x-access-token | The access token is provisioned for you by Ryng. If you do not have this already, please reach out to your Ryng project champion. Alternatively, you can raise a request on our [support portal](https://cod.atlassian.net/servicedesk/customer/portal/2). |

**Params**

For this API, params are either added by `query` params or can be given inside the request `body`&#x20;

Type:  _query params/body_

<table><thead><tr><th width="204">Key</th><th width="81">Type</th><th width="393">Description</th><th>Required?</th></tr></thead><tbody><tr><td>agentID</td><td>int</td><td>Agent to be assigned for the call</td><td>Yes</td></tr><tr><td>campaignID</td><td>int</td><td>The campaign to which the campaign is added</td><td>Yes </td></tr><tr><td>customerName</td><td>text</td><td>Name of the customer to be called</td><td>No</td></tr><tr><td>terminalCallbackUrl</td><td>string</td><td> Optional URL endpoint to receive callbacks on the terminal state of each call (i.e., each row of customer data) in this request. One callback will be sent per call. The callback will contain all metadata and disposition details for that particular call and will be sent as a POST request to this endpoint. The format of the callback is given <a href="make-preview-call.md#connect-callback-body-example">below</a>. The API expects to receive a 200 OK response from the callback URL endpoint upon successful receipt of the callback. On any non-200 response, Ryng will retry the callback at 1-minute intervals up to two more times.</td><td>No</td></tr><tr><td>connectCallbackUrl</td><td>string</td><td> Optional URL endpoint to receive callbacks on the terminal state of each call (i.e., each row of customer data) in this request. One callback will be sent per call. The callback will contain all metadata and disposition details for that particular call and will be sent as a POST request to this endpoint. The format of the callback is given <a href="make-preview-call.md#connect-callback-body-example">below</a>. The API expects to receive a 200 OK response from the callback URL endpoint upon successful receipt of the callback. On any non-200 response, Ryng will retry the callback at 1-minute intervals up to two more times.</td><td>No</td></tr></tbody></table>

## Example Body

```json
{
    "agentID": 7,
    "campaignID": 595,
    "customerNumber": "+91XXXXXXXX85",
    "customerName": "Joe"
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
    "message": "Call Initiated With SID z7a81-01uy-cydty",
    "data": {
        "callID": 16703,
        "logID": 87288
    }
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
    "message": "Failed to start call."
}
```
{% endtab %}
{% endtabs %}

### Connect Callback Body Example

```json
{
    "callID": 131,
    "callbackType": "connect",
    "event": "agent-connected",
    "agentID": 67136,
    "agentUsername": "ashwin@example.com",
    "agentNum": "+919999999999",
    "customerNum": "+91XXXXXXXX85",
    "callbackType" : "connect",
    "event":"customer-connected",
    "callCreated": "2023-02-01T18:13:18.000Z",
    "callStarted": null,
    "callDirection": "outbound",
    "customFieldNames": {
        "customField1Name": "customer name",
        "customField2Name": "meta data 1",
        "customField3Name": "meta data 2",
        "customField4Name": "meta data 3",
        "customField5Name": "meta data 4",
        "customField6Name": "meta data 5",
        "customField7Name": "meta data 6",
        "customField8Name": "meta data 7"
    },
    "customFields": {
        "customField1": "ashwin",
        "customField2": "data 1",
        "customField3": "data 2",
        "customField4": "data 3",
        "customField5": "data 4",
        "customField6": "data 5",
        "customField7": "data 6",
        "customField8": "data 7"
    }
}
```

### Terminal Callback Body Example

```json
{
    "callID": 28,
    "callLogID": 64,
    "callbackType": "terminal",
    "event": "call-ended"
    "agentID": 67131,
    "agentUsername": "ashwin@example.com",
    "agentNum": "+91999999999",
    "customerNum": "+91XXXXXXXX85",
    "callbackType" : "terminal",
    "event":"call-ended",
    "virtualNum": "+914954389444",
    "callCreated": "2023-01-18T10:08:36.000Z",
    "callStarted": "2023-01-19T08:07:07.000Z",
    "callEnded": "2023-01-19T08:12:52.000Z",
    "callDisposed": "2023-01-19T08:13:00.000Z",
    "callbackTime": null,
    "callDirection": "outbound",
    "talktime": 5,
    "callDuration": null,
    "parentDisposition": null,
    "disposition": "New Disposition",
    "notes": "",
    "status": "2",
    "recordingUrl": "https://s3-ap-southeast-1.amazonaws.com/exotelrecordings/example1/2af2864f323c31e1da22c2e82d78178s.mp3",
    "legDetails": { "leg0Status": 5, "leg0Duration": 16, "leg1Status": 5, "leg1Duration": 5 },
    "customFieldNames": {
        "customField1Name": "customer name",
        "customField2Name": "custfield1",
        "customField3Name": "custfield2",
        "customField4Name": "custfield3",
        "customField5Name": "meta data 4",
        "customField6Name": "meta data 5",
        "customField7Name": "meta data 6",
        "customField8Name": "meta data 7"
    },
    "customFields": {
        "customField1": "Test call",
        "customField2": "https://www.google.com/search?q=instagram+login&rlz=1C1CHBF_enIN1025IN1025&oq=ins&aqs=chrome.0.69i59j0i131i433i512j69i57j0i131i433i512l3j69i65j69i60.3446j0j7&sourceid=chrome&ie=UTF-8",
        "customField3": "custfield",
        "customField4": "data 7",
        "customField5": "https://www.google.com/search?q=instagram+login&rlz=1C1CHBF_enIN1025IN1025&oq=ins&aqs=chrome.0.69i59j0i131i433i512j69i57j0i131i433i512l3j69i65j69i60.3446j0j7&sourceid=chrome&ie=UTF-12",
        "customField6": "data 9",
        "customField7": "data 10",
        "customField8": "data 11"
    }
}
```
