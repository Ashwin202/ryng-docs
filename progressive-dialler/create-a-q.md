# Create a Q

## **Description:**

This endpoint is used to create a new Q on Ryng. This endpoint creates a new Q and also optionally adds customer data to the Q. Customer Data can be added as JSON, or can also be pulled from an existing Campaign. Agent assignment can also be performed either by specifying a preset mapping, or by choosing a round-robin assignment mechanism across selected agents. You can also use callbacks to get data back from Ryng for every call that is attempted.

**URL**

`https://`_`<subdomain>`_`.ryng.in/rest-api/v1/progressive/queue`

* Replace _`<subdomain>`_ with your account subdomain.

**Method**

**`POST`**

**Headers**

| Key            | Description                                                                                                                                                                                                                                               |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x-access-token | The access token is provisioned for you by Ryng. If you do not have this already, please reach out to your Ryng project champion. Alternatively you can raise a request on our [support portal](https://cod.atlassian.net/servicedesk/customer/portal/2). |

**Body**

Type:  _json_

<table><thead><tr><th width="192">PARAMETER</th><th width="115">TYPE</th><th width="355">DESCRIPTION</th><th width="183">REQUIRED</th></tr></thead><tbody><tr><td>name</td><td>String</td><td>Give a custom name for the queue. This can be leveraged to indicate the source of lead or a unique identifier like lead# from your CRM.If not specified, default value is “queue::&#x3C;timestamp>”</td><td>No</td></tr><tr><td>description</td><td>String</td><td>Descrption of queue. It can be something specific to the queue’s purpose</td><td>No</td></tr><tr><td>agentMapping</td><td>String</td><td>Specify how agents should be assigned to queue calls <br>• global - The agents to be assigned will be defined (by username) in the agents parameter  <br>• inline - Agents to be assigned for calls are fetched from the agentsAssigned parameter within jsonData, or from within the selected campaign <br>• all-agents - (default) All agents are available to be assigned for queue calls</td><td></td></tr><tr><td>agents</td><td>Array of Strings</td><td>Specify one or more agent usernames amongst whom the leads/customers should be assigned</td><td>Yes - if agentMapping set to implicit</td></tr><tr><td>legDirection</td><td>String</td><td>Should we connect to the agent first, or the customer? <br>• agent-first - Agent gets the call before the customer does<br> • customer-first - Customer gets the call before the agent does <br><br><em>Default setting fetched from tenant default queue settings if not specified</em></td><td></td></tr><tr><td>maxRetries</td><td>Number</td><td>Maximum number of retries to be made to customers who are unreachable or busy<br><em>Default setting fetched from tenant default queue settings if not specified</em></td><td>No</td></tr><tr><td>retryInterval</td><td>Number</td><td>Minimum interval (in minutes) between retries for unreachable customersDefault setting fetched from tenant default queue settings if not specified</td><td>No</td></tr><tr><td>maxDisposeTime</td><td>Number</td><td>Time allotted (in seconds) for disposing a call before the next call is initiatedDefault setting fetched from tenant default queue settings if not specified</td><td>No</td></tr><tr><td>outputToLSQ</td><td>Boolean</td><td>Set this flag if you would like us to send call-related updates to your LeadSquared CRM, as a custom activity called "Call via Ryng" (Note: This requires that your Ryng is already integrated with your LeadSquared account. Reach out to our support team to get this configured.)</td><td>No</td></tr><tr><td>customFieldNames</td><td>Object</td><td>Header names for custom fields. Object keys should be of the format customField{number}Name ex. {         "customField1Name": "Name",         "customField2Name": "Brand",         "customField3Name": "Product" }</td><td>No</td></tr><tr><td>customerData</td><td>Object</td><td>Contains customer data to be uploaded to queue. Check <a href="https://www.dropbox.com/scl/fi/8ibetw77igdnxo8b32u0t/Ryng-Q-API.paper?dl=0&#x26;rlkey=f9m71bj075k9bsddmbhb3bg9m#:uid=440143879661952064216809&#x26;h2=Customer-Data-Examples">CUSTOMER DATA EXAMPLES </a>section for examples • campaignID - (number) get call data from specified campaign. If agentMapping is set to inline then agent list is also fetched from specified campaign. See example <a href="https://www.dropbox.com/scl/fi/8ibetw77igdnxo8b32u0t/Ryng-Q-API.paper?dl=0&#x26;rlkey=f9m71bj075k9bsddmbhb3bg9m#:uid=440143879661952064216809&#x26;h2=Customer-Data-Examples">here</a>. <br>• terminalCallbackUrl - (string) Optional URL endpoint to receive callbacks on the terminal state of each call (i.e., each row of customer data) in this request. One callback will be sent per call. The callback will contain all metadata and disposition details for that particular call, and will be sent as a POST request to this endpoint. Format of the callback is given <a href="https://www.dropbox.com/scl/fi/8ibetw77igdnxo8b32u0t/Ryng-Q-API.paper?dl=0&#x26;rlkey=f9m71bj075k9bsddmbhb3bg9m#:uid=440736487211495092391008&#x26;h2=Callback-Body-Example">below</a>. The API expects to receive a 200 OK response from the callback URL endpoint upon successful receipt of callback. On any non-200 response, Ryng will retry the callback at 1 minute intervals up to two more times. <br>• connectCallbackUrl - (string) Optional URL endpoint to trigger a callback whenever an individual call (customer) is assigned to an agent. A single callback will be sent as a POST request to this endpoint each time a call is assigned to an agent. The callback will contain all metadata for that particular call in the body, the format of which is given below. The API expects to receive a 200 OK response from the callback URL endpoint. On any non-200 response, Ryng will retry the callback after a 1 minute interval one more time. <br>• jsonData - (json) Call data is specified as an array of call objects      <br>Default Fields      <br>→ number - (Required) (String) Customer number to call for a  given call     <br> → priority  - (Number) Number based priority of the call. Bigger the number, higher the priority      <br>→ agentsAssigned - (Array of Strings) Comma separated list of assignable agents to a specific call     <br> → customField{Number} - (ex. customField1) (String) Custom field values for a given</td><td>Yes</td></tr></tbody></table>

## Example Body

***

```jsx
{
    "name": "",
    "description": "",
    "agentMapping": "global"/"inline"/"all-agents", 
    "agents": ["agent1@example.com", "agent2@aexample.com"],
    "legDirection": "agent-first"/"customer-first",
    "maxRetries": 2,
    "retryInterval": 10,
    "maxDisposeTime": 30,
    "outputToLSQ":false,
    "customFieldNames": {
        "customField1Name": "Name",
        "customField2Name": "Brand",
        "customField3Name": "Product"
    },
    "customerData": {
        "campaignID": 24,
        "terminalCallbackUrl": "http://api.example.com/call-data",
        "connectCallbackUrl": "http://api.example.com/connect-data",
        "jsonData": [{ 
          "number": "+919876543210",
          "priority": 1,
          "agentsAssigned": ["agent1@example.com", "agent2@aexample.com"],
          "customField1": "Benji"
        },{
          "number": "+919876543210",
          "priority": 10,
          "agentsAssigned": ["agent1@example.com"],
          "customField1": "Benji"
        },{
          "number": "+919876543210",
          "priority": 90,
          "agentsAssigned": ["agent2@aexample.com"],
          "customField1": "Benji"
        }]
    }
}
```

## Customer Data Examples

***

#### **Call Upload From** **Existing Campaign**

```jsx
"customerData": {
  "campaignID": 24
}
```

#### **Call Upload From JSON Data**

```json
"customerData": {
    "terminalCallbackUrl": "http://api.example.com/call-data",
    "connectCallbackUrl": "http://api.example.com/connect-data",
    "jsonData": [{ 
      "number": "+919876543210",
      "priority": 1,
      "agentsAssigned": ["agent1@example.com", "agent2@aexample.com"],
      "customField1": "Benji"
    },{ 
      "number": "+919876543210",
      "priority": 10,
      "agentsAssigned": ["agent1@example.com"],
      "customField1": "Benji"
    },{
      "number": "+919876543210",
      "priority": 90,
      "agentsAssigned": ["agent2@aexample.com"],
      "customField1": "Benji"
    }]
}
```

## API Responses

## Create Q API

<mark style="color:green;">`POST`</mark> `https://tenant.ryng.in/rest-api/v1/progressive/queue`

#### Headers

| Name                                             | Type   | Description |
| ------------------------------------------------ | ------ | ----------- |
| x-access-token<mark style="color:red;">\*</mark> | String |             |

{% tabs %}
{% tab title="200: OK Success" %}
```json
{
    "error": false,
    "message": "Queue Created! Processing Calls...",
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
    "message": "Failed to Create Queue"
}
```
{% endtab %}
{% endtabs %}

***

### Terminal Callback Body Example

```jsx
{
    "queueID": 2,
    "callID": 245,
    "callLogID": 894,
    "agentID": 2,
    "agentUsername": "ben@askerbot.com",
    "agentNum": "+918714218838",
    "customerNum": "+919645571209",
    "virtualNum": "02235510003",
    "callbackType" : "terminal",
    "event":"call-ended",
    "callCreated": "2020-11-18T07:10:00.000Z",
    "callStarted": "2020-11-18T09:00:14.000Z",
    "callEnded": "2020-11-18T14:34:57.000Z",
    "callDisposed": "2020-11-18T14:34:57.000Z",
    "callbackTime": "2021-08-12T12:04:34.00Z",
    "scheduledDate": "2020-09-02T00:11:55.00Z",
    "callDirection": "outbound",
    "legDirection": "agent-first",
    "talktime": 27,
    "callDuration": 32,
    "parentDisposition": "Customer Accepted",
    "disposition": "Customer requested forms",
    "notes": "Customer Accepted",
    "status": "completed",
    "recordingUrl": "https://example.com/recordings/sid/ash4jks8mrji8043.mp3",
    "legDetails": [
        "leg0Status": "completed",
        "leg0Duration": 27,
        "leg1Status": "completed",
        "leg1Duration": 24,
    ]
    "customFieldNames": {
        "customField1Name": "Name",
        "customField2Name": "Brand",
        "customField3Name": "Product"
    },
    "customFields": {
        "customField1": "Extra cool",
        "customField2": "Close UP",
        "customField3": "Toothpaste"
    }
}
```

### Connect Callback Body Example

```jsx
{
    "queueID": 2,
    "queueName": "Standard Queue",
    "callID": 245,
    "agentID": 2,
    "agentUsername": "agent@askerbot.com",
    "agentNum": "+918714218838",
    "customerNum": "+919645571209",
    "callbackType" : "connect",
    "event":"customer-connected",
    "callCreated": "2021-12-15T13:06:33.000Z",
    "callStarted": "2021-12-15T13:06:41.000Z",
    "callDirection": "outbound",
    "legDirection": "agent-first",
      "customFieldNames": {
        "customField1Name": "Name",
        "customField2Name": "Brand",
        "customField3Name": "Product"
    },
    "customFields": {
        "customField1": "Extra cool",
        "customField2": "Close UP",
        "customField3": "Toothpaste"
    }
}
```
