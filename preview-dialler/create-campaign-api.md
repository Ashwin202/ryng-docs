# Create Campaign API

## Pre-Requisites

To get started ensure you have the following checklist:

1. An access token issued by Ryng
2. A list of all agents & their IDs/usernames.

## API Definitions and Parameters

**URL**

`https://`_`<subdomain>`_`.ryng.in/rest-api/v1/campaign`

* Replace _`<subdomain>`_ with your account subdomain.

**Method**

**`POST`**

**Headers**

| Key            | Description                                                                                                                                                                                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x-access-token | The access token is provisioned for you by Ryng. If you do not have this already, please reach out to your Ryng project champion. Alternatively, you can raise a request on our [support portal](https://cod.atlassian.net/servicedesk/customer/portal/2). |

**Body**

For this API, body parameters are accepted in either `form-data` or `JSON` format. You can choose the format that best suits your application's requirements or preferences. Note: In order to upload a csv sheet using the `campaign-sheet` param, you will need to use `form-data`, since this does not work over `JSON`

Type:  _form-data_

<table><thead><tr><th width="196">Key</th><th width="92">Type</th><th width="317">Description</th><th>Required?</th></tr></thead><tbody><tr><td>campaign-sheet</td><td>file</td><td>Attach a .csv file with your lead/customer information that is to be assigned to your agents</td><td>Yes</td></tr><tr><td>agents</td><td>text</td><td>Specify one or more agents amongst whom the leads/customers should be assigned, or else, use “in-file” to provide agent mapping within the CSV file itself. If not specified, default value = all agents</td><td>Yes - if only one lead, or for in-file mapping.<br>No - if more than one lead</td></tr><tr><td>campaign-name</td><td>text</td><td>Give a custom name for your campaign. This can be leveraged to indicate the source of lead or a unique identifier like lead# from your CRM. If not specified, Ryng will give random campaign name.</td><td>No</td></tr></tbody></table>

Type: JSON

<table><thead><tr><th width="198">PARAMETER</th><th width="94">TYPE</th><th width="346">DESCRIPTION</th><th width="183">REQUIRED</th></tr></thead><tbody><tr><td>name</td><td>String</td><td>Give a custom name for the campaign. This can be leveraged to indicate the source of lead or a unique identifier like lead# from your CRM. If not specified, the default value is “campaign::&#x3C;campaignID>”</td><td>No</td></tr><tr><td>agentMapping</td><td>String</td><td>Specify how agents should be assigned to campaign calls <br>• global - The agents to be assigned will be defined (by username) in the agents parameter  <br>• inline - Agents to be assigned for calls are fetched from the agentsAssigned parameter within jsonData, or from within the selected campaign <br>• all-agents - (default) All agents are available to be assigned for campaign calls</td><td></td></tr><tr><td>agents</td><td>Array of Strings</td><td>Specify one or more agent usernames amongst whom the leads/customers should be assigned</td><td>Yes - if agentMapping set to implicit</td></tr><tr><td>customFieldNames</td><td>Object</td><td>Header names for custom fields. Object keys should be of the format customField{number}Name ex. {         "customField1Name": "Name",         "customField2Name": "Brand",         "customField3Name": "Product" }</td><td>No</td></tr><tr><td>customerData</td><td>Object</td><td><p>Contains customer data to be uploaded to campaign. Check <a href="create-campaign-api.md#example-body">example body</a> section for the example</p><p>→ connectCallbackUrl - Optional URL endpoint to trigger a callback at the major milestones of a call lifecycle. These milestones are: agent picks up, customer picks up, and call termination. Ryng will trigger one callback at each of these stages. The callback will contain all metadata for that particular call in the body, the format of which is given <a href="create-campaign-api.md#connect-callback-body-example">below</a>. The API expects to receive a 200 OK response from the callback URL endpoint. On any non-200 response, Ryng will retry the callback after a 1 minute interval one more time.</p><p>→ terminalCallbackUrl - Optional URL endpoint to receive callbacks on the terminal state of each call (i.e., each row of customer data) in this request. One callback will be sent per call. The callback will contain all metadata and disposition details for that particular call, and will be sent as a POST request to this endpoint. Format of the callback is given <a href="create-campaign-api.md#terminal-callback-body-example">below</a>. The API expects to receive a 200 OK response from the callback URL endpoint upon successful receipt of the callback. On any non-200 response, Ryng will retry the callback at 1-minute intervals up to two more times.<br>→ jsonData - (json) Call data is specified as an array of call objects      <br>Default Fields      <br>→ number - (Required) (String) Customer number to call for a  given call      <br>→ agentsAssigned - (Array of Strings) Comma separated list of assignable agents to a specific call     <br> → customField{Number} - (ex. customField1) (String) Custom field values for a given</p></td><td>Yes</td></tr></tbody></table>

## Example Body

```json
{
  "name": "Example Campaign",
  "agentMapping": "global"/"inline"/"all-agents",
  "agents": ["ashwin@example.com", "alice@example.com"],
  "customFieldNames": {
    "customField1Name": "Name",
    "customField2Name": "Brand",
    "customField3Name": "Product"
  },
  "customerData": {
    "terminalCallbackUrl": "http://api.example.com/call-data",
    "connectCallbackUrl": "http://api.example.com/connect-data",
    "jsonData": [
      {
        "number": "+919876543210",
        "agentsAssigned": ["john@example.com", "emma@example.com"],
        "customField1": "Max"
      },
      {
        "number": "+917994771185",
        "agentsAssigned": ["agent3@example.com"],
        "customField1": "Alex"
      },
      {
        "number": "+919876543210",
        "agentsAssigned": ["agent1@example.com"],
        "customField1": "Sophia"
      },
      {
        "number": "+919876543210",
        "agentsAssigned": ["agent2@example.com"],
        "customField1": "Oliver"
      }
    ]
  }
}

```

## API Responses

## Create Campaign API

<mark style="color:green;">`POST`</mark> `https://tenant.ryng.xyz/rest-api/v1/campaign`

#### Headers

| Name                                             | Type | Description            |
| ------------------------------------------------ | ---- | ---------------------- |
| x-access-token<mark style="color:red;">\*</mark> |      | Provided by Ryng Team  |

{% tabs %}
{% tab title="200: OK Success Response" %}
```json
{
    "error": false,
    "message": "Processing Campaign...",
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
    "message": "Failed to create Campaign"
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
    "customerNum": "+917777445511",
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
    "callID": 19,
    "callLogID": 49,
    "callbackType": "terminal",
    "event": "call-ended"
    "agentID": 67131,
    "agentUsername": "ashwin@example.com",
    "agentNum": "+91999999999",
    "customerNum": "+917445662236",
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

## Case Study: Using Campaign API in transactional nature

The Campaign API is best suited for campaigns with more than one record, where you are sending records in bulk to Ryng. However, if you are trying to leverage this API in a transactional nature (that is a campaign with a single record) it is essential that you specify the agent for assignment.

{% hint style="warning" %}
If you are trying to leverage this API in a transactional nature, it is essential that you specify the agent for assignment.
{% endhint %}

For example, if you have the following agents:

| Agent.id | Agent.username  |
| -------- | --------------- |
| 10       | john@icloud.com |
| 20       | tom@apple.in    |
| 30       | tim@cook.com    |

And you have data flowing in as seen below:

**10AM:**

| Mobile    | Customer Name | Policy Interested | Lead ID |
| --------- | ------------- | ----------------- | ------- |
| 998314242 | James         | Life Insurance    | XT243   |

**10:15 AM:**

| Mobile    | Customer Name | Policy Interested | Lead ID |
| --------- | ------------- | ----------------- | ------- |
| 893247489 | Philip        | Motor Insurance   | XT250   |

If you were to post this to Ryng via the Campaign API without specifying agents, it would always assign both leads to Agent.id = 10 (i.e your first agent). This happens since the round-robin assignment is currently applicable only within a campaign, and not across.

To work around this, simply specify an agent id from your list of agents (provided by us), within your POST request.

In this scenario, your body for each request would look like:

10AM:

| Parameter      | Value     |
| -------------- | --------- |
| campaign-sheet | xt243.csv |
| agent          | 10        |

10:15 AM:

| Parameter      | Value     |
| -------------- | --------- |
| campaign-sheet | xt250.csv |
| agent          | 20        |
