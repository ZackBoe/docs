---
title: IronMQ REST/HTTP API
layout: default
section: mq
breadcrumbs:
  - ['Reference', '/reference']
  - ['REST/HTTP API', '/api']
---

IronMQ provides a REST/HTTP API to allow you to interact programmatically with your queues on IronMQ.

<section id="toc">
  <h3>Table of Contents</h3>
  <ul>
    <li>[Endpoints](#endpoints)</li>
    <li>[Authentication](#authentication)</li>
    <li>
      [Requests](#requests)
      <ul>
        <li>[Base URL](#base_url)</li>
        <li>[Pagination](#pagination)</li>
      </ul>
    </li>
    <li>
      [Responses](#responses)
      <ul>
        <li>[Status Codes](#status_codes)</li>
        <li>[Exponential Backoff](#exponential_backoff)</li>
      </ul>
    </li>
  </ul>
</section>

## <a name="endpoints"></a> Endpoints

<table class="reference">
  <thead>
    <tr><th style="width: 58%;">URL</th><th style="width: 10%;">HTTP Verb</th><th style="width: 32%;">Purpose</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues</td>
      <td>GET</td>
      <td><a href="#list_message_queues" title="List Message Queues">List Message Queues</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span></td>
      <td>GET</td>
      <td><a href="#get_info_about_a_message_queue" title="Get Info About a Message Queue">Get Info About a Message Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span></td>
      <td>POST</td>
      <td><a href="#update_a_message_queue" title="Update a Message Queue">Update a Message Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span></td>
      <td>DELETE</td>
      <td><a href="#delete_a_message_queue" title="Delete a Message Queue">Delete a Message Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/clear</td>
      <td>POST</td>
      <td><a href="#clear_all_messages_from_a_queue" title="Clear All Messages from a Queue">Clear All Messages from a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages</td>
      <td>POST</td>
      <td><a href="#add_messages_to_a_queue" title="Add Messages to a Queue">Add Messages to a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages/webhook</td>
      <td>POST</td>
      <td><a href="#add_messages_to_a_queue_via_webhook" title="Add Messages to a Queue via Webhook">Add Messages to a Queue via Webhook</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages</td>
      <td>GET</td>
      <td><a href="#get_messages_from_a_queue" title="Get Messages from a Queue">Get Messages from a Queue</a></td>
    </tr>
    <!--
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages/peek</td>
      <td>GET</td>
      <td><a href="#peek_messages_on_a_queue" title="Peek Messages on a Queue">Peek Messages on a Queue</a></td>
    </tr>
    -->
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span></td>
      <td>GET</td>
      <td><a href="#get_message_by_id" title="Get Message by ID">Get Message by ID</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span></td>
      <td>DELETE</td>
      <td><a href="#delete_a_message_from_a_queue" title="Delete a Message from a Queue">Delete a Message from a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages</td>
      <td>DELETE</td>
      <td><a href="#delete_multiple_messages_from_a_queue" title="Delete Multiple Messages from a Queue">Delete Multiple Messages from a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>/touch</td>
      <td>POST</td>
      <td><a href="#touch_a_message_on_a_queue" title="Touch a Message on a Queue">Touch a Message on a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>/release</td>
      <td>POST</td>
      <td><a href="#release_a_message_on_a_queue" title="Release a Message on a Queue">Release a Message on a Queue</a></td>
    </tr>
  </tbody>
</table>

#### Related to Pull Queues

<table class="reference">
  <thead>
    <tr>
      <th style="width: 58%;">URL</th>
      <th style="width: 10%;">HTTP Verb</th>
      <th style="width: 32%;">Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/alerts</td>
      <td>POST</td>
      <td><a href="#add_alerts_to_a_queue" title="Add Alerts to a Queue">Add Alerts to a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/alerts</td>
      <td>PUT</td>
      <td><a href="#replace_alerts_on_a_queue" title="Update Alerts on a Queue">Replace Alerts on a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/alerts</td>
      <td>DELETE</td>
      <td><a href="#remove_alerts_from_a_queue" title="Remove Alerts from a Queue">Remove Alerts from a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/alerts/<span class="variable alert_id">{Alert ID}</span></td>
      <td>DELETE</td>
      <td><a href="#remove_alert_from_a_queue_by_id" title="Remove Alert from a Queue by ID">Remove Alert from a Queue by ID</a></td>
    </tr>
  </tbody>
</table>

#### Related to Push Queues

<table class="reference">
  <thead>
    <tr>
      <th style="width: 58%;">URL</th>
      <th style="width: 10%;">HTTP Verb</th>
      <th style="width: 32%;">Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/subscribers</td>
      <td>POST</td>
      <td><a href="#add_subscribers_to_a_queue" title="Add Subscribers to a Queue">Add Subscribers to a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/subscribers</td>
      <td>DELETE</td>
      <td><a href="#remove_subscribers_from_a_queue" title="Remove Subscribers from a Queue">Remove Subscribers from a Queue</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>/subscribers</td>
      <td>GET</td>
      <td><a href="#get_push_status_for_a_message" title="Get Push Status for a Message">Get Push Status for a Message</a></td>
    </tr>
    <tr>
      <td>/projects/<span class="project_id variable">{Project ID}</span>/queues/<span class="queue_name variable">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>/subscribers/<span class="subscriber_id variable">{Subscriber ID}</span></td>
      <td>DELETE</td>
      <td><a href="#acknowledge__delete_push_message_for_a_subscriber" title="Delete Push Message for a Subscriber">Delete Push Message for a Subscriber</a></td>
    </tr>
  </tbody>
</table>


## <a name="authentication"></a>Authentication
IronMQ uses OAuth2 tokens to authenticate API requests. All methods require authentication unless specified otherwise. You can find and create your API tokens [in the HUD](https://hud.iron.io/tokens). To authenticate your request, you should include a token in the Authorization header for your request or in your query parameters. Tokens are universal, and can be used across services.

Note that each request also requires a Project ID to specify which project the action will be performed on. You can find your Project IDs [in the HUD](https://hud.iron.io). Project IDs are also universal, so they can be used across services as well.

**Example Authorization Header**:
Authorization: OAuth abc4c7c627376858

**Example Query with Parameters**:
GET https://<span class="variable host">mq-aws-us-east-1</span>.iron.io/1/projects/<span class="variable project_id">{Project ID}</span>/queues?oauth=abc4c7c627376858

Notes:

* Be sure you have the correct case, it's **OAuth**, not Oauth.
* In URL parameter form, this will be represented as:
        `?oauth=abc4c7c627376858`

## <a name="requests"></a>Requests

Requests to the API are simple HTTP requests against the API endpoints.

All request bodies should be in JSON format, with Content-Type of `application/json`.

<h3 id="base_url">Base URL</h3>

All endpoints should be prefixed with the following:

<strong>https://<span class="variable domain project_id">{Host}</span>.iron.io/<span class="variable version project_id">{API Version}</span>/</strong>

<strong>API Version Support</strong> : IronMQ API supports version <strong>1</strong>


The hosts for the clouds IronMQ supports are as follows:

<table class="reference">
  <thead>
    <tr>
      <th>Cloud</th>
      <th>Host</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AWS US-EAST</td>
      <td>mq-aws-us-east-1.iron.io</td>
    </tr>
    <tr>
      <td>AWS EU-WEST</td>
      <td>mq-aws-eu-west-1.iron.io</td>
    </tr>
    <tr>
      <td>Rackspace ORD</td>
      <td>mq-rackspace-ord.iron.io</td>
    </tr>
    <tr>
      <td> Rackspace LON </td>
      <td>mq-rackspace-lon.iron.io</td>
    </tr>
    <tr>
      <td>Rackspace DFW</td>
      <td>Pro Plans Only - <a href="mailto:support@iron.io?subject=Iron.io%20Dev%20Center%20Question&amp;body=(please%20select%20those%20that%20apply%20to%20you)%0AI%20am%20asking%20a%20question%20about%20IronMQ%2FIronWorker%2FIronCache%20%0ALanguage%3A%0AQuestion%3A%0ALinks%3A" target="_blank">Email Support</a></td>
    </tr>
  </tbody>
</table>

[Alternative domains can be found here in case of dns failures](https://plus.google.com/u/0/101022900381697718949/posts/36PS2dQH3Gn).

<h3 id="pagination">Pagination</h3>

For endpoints that return lists/arrays of values:

* page - The page of results to return. Default is 0. Maximum is 100.
* per_page - The number of results to return. It may be less if there aren't enough results. Default is 30. Maximum is 100.

## <a name="responses"></a>Responses

All responses are in JSON, with Content-Type of `application/json`. A response is structured as follows:

```js
{ "msg": "some success or error message" }
```

<h3 id="status_codes">Status Codes</h3>
The success failure for request is indicated by an HTTP status code. A 2xx status code indicates success, whereas a 4xx status code indicates an error.

<table class="reference">
    <thead>
        <tr>
            <th style="width: 10%">Code</th><th style="width: 90%">Status</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>200</td><td>OK: Successful GET</td>
        </tr>
        <tr>
            <td>201</td><td>Created: Successful POST</td>
        </tr>
        <tr>
            <td>400</td><td>Bad Request: Invalid JSON (can't be parsed or has wrong types).</td>
        </tr>
        <tr>
            <td>401</td><td>Unauthorized: The OAuth token is either not provided or invalid.</td>
        </tr>
        <tr>
            <td>403</td><td>Project suspected, resource limits.</td>
        </tr>
        <tr>
            <td>404</td><td>Not Found: The resource, project, or endpoint being requested doesn’t exist.</td>
        </tr>
        <tr>
            <td>405</td><td>Invalid HTTP method: A GET, POST, DELETE, or PUT was sent to an endpoint that doesn’t support that particular verb.</td>
        </tr>
        <tr>
            <td>406</td><td>Not Acceptable: Required fields are missing.</td>
        </tr>
        <tr>
            <td>503</td><td>Service Unavailable. Clients should implement <a href="#exponential_backoff">exponential backoff</a> to retry the request.</td>
        </tr>
    </tbody>
</table>

Specific endpoints may provide other errors in other situations.

When there's an error, the response body contains a JSON object something like:

```js
{ "msg": "reason for error" }
```

<h3 id="exponential_backoff">Exponential Backoff</h3>

When a 503 error code is returned, it signifies that the server is currently unavailable. This means there was a problem processing the request on the server-side; it makes no comment on the validity of the request. Libraries and clients should use [exponential backoff](http://en.wikipedia.org/wiki/Exponential_backoff) when confronted with a 503 error, retrying their request with increasing delays until it succeeds or a maximum number of retries (configured by the client) has been reached.

## <a name="list_message_queues"></a> List Message Queues

Get a list of all queues in a project. By default, 30 queues are listed at a time. To see more, use the `page` parameter or the `per_page` parameter. Up to 100 queues may be listed on a single page.

### Endpoint

<div class="grey-box">
GET /projects/<span class="variable project_id">{Project ID}</span>/queues
</div>

#### URL Parameters

* **Project ID**: Project these queues belong to

#### Optional URL Parameters

* **page**: The 0-based page to view. The default is 0.
* **per_page**: The number of queues to return per page. The default is 30, the maximum is 100.

### Response

```js
[
  {
    "id": "1234567890abcdef12345678",
    "project_id": "1234567890abcdef12345678",
    "name": "queue name"
  }
]
```


## <a name="get_info_about_a_message_queue"></a> Get Info About a Message Queue


This call gets general information about the queue.

### Endpoint

<div class="grey-box">
GET /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>
</div>

#### URL Parameters

* **Project ID**: Project the queue belongs to
* **Queue Name**: Name of the queue

### Response
```js
{
  "size": "queue size"
}
```

## <a name="delete_a_message_queue"></a> Delete a Message Queue

This call deletes a message queue and all its messages.

### Endpoint

<div class="grey-box">
DELETE /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>
</div>

#### URL Parameters

* **Project ID**: Project the queue belongs to
* **Queue Name**: Name of the queue

### Response
```js
{
  "msg": "Deleted."
}
```

## <a name="update_a_message_queue"></a> Update a Message Queue

This allows you to change the properties of a queue including setting subscribers and the push type if you want it to be a
push queue.

### Endpoint

<div class="grey-box">
POST /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>
</div>

#### URL Parameters

* **Project ID**: Project the queue belongs to
* **Queue Name**: Name of the queue. If the queue does not exist, it will be created for you.

<div class="grey-box">
<strong>WARNING:</strong> Do not use the following RFC 3986 Reserved Characters  within your in the naming of your queues.
  <p>! * ' ( ) ; : @ & = + $ , / ? # [ ]</p>
</div>
#### Body Parameters

##### Optional

The following parameters are all related to Push Queues.

* **subscribers**: An array of subscriber hashes containing a required "url" field and an optional "headers" map for custom headers. This set of subscribers will replace the existing subscribers. See [Push Queues](/mq/reference/push_queues/) to learn more about types of subscribers. To add or remove subscribers, see the <a href="#add_subscribers_to_a_queue">add subscribers endpoint</a> or the <a href="#remove_subscribers_from_a_queue">remove subscribers endpoint</a>. The maximum is 64kb for JSON array of subscribers' hashes. See below for example JSON.
* **push_type**: Either `multicast` to push to all subscribers or `unicast` to push to one and only one subscriber.
Default is `multicast`. To revert push queue to reqular pull queue set `pull`.
* **retries**: How many times to retry on failure. Default is 3. Maximum is 100.
* **retries_delay**: Delay between each retry in seconds. Default is 60.
* **error_queue**: The name of another queue where information about messages that can't be delivered after retrying `retries` number of times will be placed. Pass in an empty string to disable error queues. Default is disabled. See [Push Queues](/mq/reference/push_queues/) to learn more.

### Request

```js
{
  "push_type":"multicast",
   "subscribers": [
     {"url": "http://mysterious-brook-1807.herokuapp.com/ironmq_push_1"},
     {
        "url": "http://mysterious-brook-1807.herokuapp.com/ironmq_push_2",
        "headers": {"Content-Type": "application/json"}
     }
   ]
}
```


### Response
```js
{
  "id":"50eb546d3264140e8638a7e5",
  "name":"pushq-demo-1",
  "size":7,
  "total_messages":7,
  "project_id":"4fd2729368a0197d1102056b",
  "retries":3,
  "push_type":"multicast",
  "retries_delay":60,
  "subscribers":[
    {"url":"http://mysterious-brook-1807.herokuapp.com/ironmq_push_1"},
    {"url":"http://mysterious-brook-1807.herokuapp.com/ironmq_push_2", "headers": {"Content-Type": "application/json"}}
  ]
}
```

## <a name="add_alerts_to_a_queue"></a> Add Alerts to a Queue

Add alerts to a queue. This is for Pull Queue only.

<div class="grey-box">
POST /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/alerts/
</div>


##### Optional

* **alerts**: An array of alert hashes containing required "type", "queue", "trigger", and optional "direction", "snooze" fields. Maximum number of alerts is 5. See [Queue Alerts](/mq/reference/queue_alerts/) to learn more.

### Request

```js
{
   "alerts": [
     {
       "type": "fixed",
       "direction": "asc",
       "trigger": 1000,
       "queue": "my_queue_for_alerts"
     }
   ]
}
```

#### Response

```js
{
  "msg": "Alerts were added."
}
```

## <a name="replace_alerts_on_a_queue"></a> Replace Alerts on a Queue

Replace current queue alerts with a given list of alerts. This is for Pull Queue only.

<div class="grey-box">
PUT /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/alerts/
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.

#### Body Parameters

##### Optional

* **alerts**: An array of alerts hashes containing required "type", "queue", "trigger", and optional "direction", "snooze" fields. Maximum number of alerts is 5. See [Queue Alerts](/mq/reference/queue_alerts/) to learn more.

### Request

```js
{
   "alerts": [
     {
       "type": "progressive",
       "direction": "desc",
       "trigger": 1000,
       "queue": "my_queue_for_alerts"
     }
   ]
}
```

Note: to clear all alerts on a queue send an empty alerts array like
so:

```js
{
  "alerts": []
}
```

#### Response

```js
{
  "msg": "Alerts were replaced."
}
```

## <a name="remove_alerts_from_a_queue"></a> Remove Alerts from a Queue

Remove alerts from a queue. This is for Pull Queue only.

<div class="grey-box">
DELETE /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/alerts/
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.

#### Body Parameters

##### Optional

* **alerts**: An array of alerts hashes containing "id" field. See [Queue Alerts](/mq/reference/queue_alerts/) to learn more.

### Request

```js
{
   "alerts": [
     {"id": "5eee546df4a4140e8638a7e5"}
   ]
}
```

#### Response

```js
{
  "msg": "Alerts were deleted."
}
```

## <a name="remove_alert_from_a_queue_by_id"></a> Remove Alert from a Queue by ID

Remove alert from a queue by its ID. This is for Pull Queue only.

<div class="grey-box">
DELETE /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/alerts/<span class="variable alert_id">{Alert ID}</span>
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.
* **Alert ID**: The id of the alert to delete.

#### Response

```js
{
  "msg": "Alerts were deleted."
}
```

## <a name="add_subscribers_to_a_queue"></a> Add Subscribers to a Queue

Add subscribers (HTTP endpoints) to a queue. This is for Push Queues only.

### Endpoint

<div class="grey-box">
POST /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}/subscribers</span>
</div>

#### URL Parameters

* **Project ID**: Project the queue belongs to
* **Queue Name**: Name of the queue. If the queue does not exist, it will be created for you.

#### Body Parameters

##### Optional

The following parameters are all related to Push Queues.

* **subscribers**: An array of subscriber hashes containing a required "url" field and an optional "headers" map for custom headers. See below for example. See [Push Queues](/mq/reference/push_queues/) to learn more about types of subscribers.

### Request

```js
{
   "subscribers": [
     {
        "url": "http://mysterious-brook-1807.herokuapp.com/ironmq_push_2",
        "headers": {"Content-Type": "application/json"}
     }
   ]
}
```


### Response

```js
{
  "id":"50eb546d3264140e8638a7e5",
  "name":"pushq-demo-1",
  "size":7,
  "total_messages":7,
  "project_id":"4fd2729368a0197d1102056b",
  "retries":3,
  "push_type":"multicast",
  "retries_delay":60,
  "subscribers":[
    {"url":"http://mysterious-brook-1807.herokuapp.com/ironmq_push_1"},
    {"url":"http://mysterious-brook-1807.herokuapp.com/ironmq_push_2", "headers": {"Content-Type": "application/json"}}
  ]
}
```

## <a name="remove_subscribers_from_a_queue"></a> Remove Subscribers from a Queue

Remove subscriber from a queue. This is for Push Queues only.

### Endpoint

<div class="grey-box">
DELETE /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}/subscribers</span>
</div>

#### URL Parameters

* **Project ID**: Project the queue belongs to
* **Queue Name**: Name of the queue. If the queue does not exist, it will be created for you.

#### Body Parameters

##### Optional

The following parameters are all related to Push Queues.

* **subscribers**: An array of subscriber hashes containing a "url" field. See below for example.

### Request

```js
{
   "subscribers": [
     {"url": "http://mysterious-brook-1807.herokuapp.com/ironmq_push_2"}
   ]
}
```


### Response

```js
{
  "id":"50eb546d3264140e8638a7e5",
  "name":"pushq-demo-1",
  "size":7,
  "total_messages":7,
  "project_id":"4fd2729368a0197d1102056b",
  "retries":3,
  "push_type":"multicast",
  "retries_delay":60,
  "subscribers":[
    {"url":"http://mysterious-brook-1807.herokuapp.com/ironmq_push_1"}
  ]
}
```

## <a name="clear_all_messages_from_a_queue"></a> Clear All Messages from a Queue

This call deletes all messages on a queue, whether they are reserved or not.

### Endpoint

<div class="grey-box">
POST /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/clear
</div>

### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of the queue.

### Response

```js
{
  "msg": "Cleared"
}
```

## <a name="add_messages_to_a_queue"></a> Add Messages to a Queue

This call adds or pushes messages onto the queue.

### Endpoint

<div class="grey-box">
POST /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of the queue. If the queue does not exist, it will be created for you.

#### Message Object

Multiple messages may be added in a single request, provided that the messages should all be added to the same queue. Each message object should contain the following keys:

##### Required

* **body**: The message data

##### Optional

* **timeout**: After timeout (in seconds), item will be placed back onto queue. You must delete the message from the queue to ensure it does not go back onto the queue. Default is 60 seconds. Minimum is 30 seconds, and maximum is 86,400 seconds (24 hours).
* **delay**: The item will not be available on the queue until this many seconds have passed. Default is 0 seconds. Maximum is 604,800 seconds (7 days).
* **expires_in**: How long in seconds to keep the item on the queue before it is deleted. Default is 604,800 seconds (7 days). Maximum is 2,592,000 seconds (30 days).

### Request

```js
{
  "messages": [
    {
      "body": "This is my message 1."
    },
    {
      "body": "This is my message 2.",
      "timeout": 30,
      "delay": 2,
      "expires_in": 86400
    }
  ]
}
```

### Response

```js
{
  "ids": ["message 1 ID", "message 2 ID"],
  "msg": "Messages put on queue."
}
```


## <a name="add_messages_to_a_queue_via_webhook"></a> Add Messages to a Queue via Webhook

By adding the queue URL below to a third party service that supports webhooks, every webhook event that the third party posts
will be added to your queue. The request body as is will be used as the "body" parameter in normal POST to queue above.

### Endpoint

<div class="grey-box">
POST /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/webhook
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of the queue. If the queue does not exist, it will be created for you.


## <a name="get_messages_from_a_queue"></a> Get Messages from a Queue

This call gets/reserves messages from the queue. The messages will not be deleted, but will be reserved until the timeout expires. If the timeout expires before the messages are deleted, the messages will be placed back onto the queue. As a result, be sure to **delete** the messages after you're done with them.

### Endpoint

<div class="grey-box">
GET /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages
</div>

#### URL Parameters

* **Project ID**: The Project these messages belong to.
* **Queue Name**: The name of queue.

#### Optional Parameters

* **n**: The maximum number of messages to get. Default is 1. Maximum is 100. Note: You may not receive all n messages on every request, the more sparse the queue, the less likely you are to receive all n messages.
* **timeout**: After timeout (in seconds), item will be placed back onto queue. You must delete the message
from the queue to ensure it does not go back onto the queue. If not set, value from POST is used. Default is 60 seconds, minimum is 30 seconds, and maximum is 86,400 seconds (24 hours).
* **wait**: Time in seconds to wait for a message to become available. This enables long polling. Default is 0 (does not wait), maximum is 30.
* **delete**: true/false. This will delete the message on get. Be careful though, only use this if you are ok with losing a message if something goes wrong after you get it. Default is false.

### Sample Request

<div class="grey-box">
GET /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages?<strong>n=10</strong>&amp;<strong>timeout=120</strong>
</div>

### Response

```js
{
  "messages": [
    {
       "id": 1,
       "body": "first message body",
       "timeout": 600,
       "reserved_count": 1,
       "push_status": {"retries_remaining": 1}
    },
    {
       "id": 2,
       "body": "second message body",
       "timeout": 600,
       "reserved_count": 1,
       "push_status": {"retries_remaining": 1}
    }
  ]
}
```

<!--
## <a name="peek_messages_on_a_queue"></a> Peek Messages on a Queue

Peeking at a queue returns the next messages on the queue, but it does not reserve them.

### Endpoint

<div class="grey-box">
GET /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/peek
</div>

#### URL Parameters

* **Project ID**: The Project these messages belong to.
* **Queue Name**: The name of queue.

#### Optional Parameters

* **n**: The maximum number of messages to peek. Default is 1. Maximum is 100. Note: You may not receive all n messages on every request, the more sparse the queue, the less likely you are to receive all n messages.

### Sample Request

<div class="grey-box">
GET /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/peek?<strong>n=10</strong>
</div>

### Response

```js
{
  "messages": [
    {
       "id": 1,
       "body": "first message body",
       "reserved_count": 0
    },
    {
       "id": 2,
       "body": "second message body",
       "reserved_count": 0
    }
  ],
}

```
-->

## <a name="get_message_by_id"></a> Get Message by ID

Get a message by ID.

### Endpoint

<div class="grey-box">
GET /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>
</div>

#### URL Parameters

* **Project ID**: The Project these messages belong to.
* **Queue Name**: The name of queue.
* **Message ID**: The id of the message to release.

### Sample Request

<div class="grey-box">
GET /projects/4ccf55250948510894024119/queues/test_queue/messages/5981787539458424851
</div>

### Response

```js
{
  "id": "5924625841136130921",
  "body": "hello 265",
  "timeout": 60,
  "status":"deleted",
  "reserved_count": 1
}
```

## <a name="release_a_message_on_a_queue"></a> Release a Message on a Queue

Releasing a reserved message unreserves the message and puts it back on the queue as if the message had timed out.

### Endpoint

<div class="grey-box">
POST /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>/release
</div>

### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.
* **Message ID**: The id of the message to release.

### Body Parameters

* **delay**: The item will not be available on the queue until this many seconds have passed. Default is 0 seconds. Maximum is 604,800 seconds (7 days).

### Request Body

```js
{
  "delay": 60
}
```

A JSON document body is required even if all parameters are omitted.

```js
{}
```

### Response
```js
{
  "msg": "Released"
}
```

## <a name="touch_a_message_on_a_queue"></a> Touch a Message on a Queue

Touching a reserved message extends its timeout to the duration specified when the message was created. Default is 60 seconds.

### Endpoint

<div class="grey-box">
POST /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>/touch
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.
* **Message ID**: The id of the message to touch.


#### Request

Any empty JSON body.

```js
{}
```

#### Response
```js
{
  "msg": "Touched"
}
```

## <a name="delete_a_message_from_a_queue"></a> Delete a Message from a Queue

This call will delete the message. Be sure you call this after you're done with a message or it will be placed back on the queue.

### Endpoint

<div class="grey-box">
DELETE /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.
* **Message ID**: The id of the message to delete.

#### Response
```js
{
  "msg": "Deleted"
}
```


## <a name="delete_multiple_messages_from_a_queue"></a> Delete Multiple Messages from a Queue

This call will delete multiple messages in one call.

### Endpoint

<div class="grey-box">
DELETE /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.

### Body Parameters

* **ids**: An array of message ids as string.

### Request Body

```js
{
  "ids": [
    "MESSAGE_ID_1",
    "MESSAGE_ID_2"
  ]
}
```


#### Response
```js
{
  "msg": "Deleted"
}
```

## <a name="get_push_status_for_a_message"></a> Get Push Status for a Message

You can retrieve the push status for a particular message which will let you know which subscribers have received the
message, which have failed, how many times it's tried to be delivered and the status code returned from the endpoint.

<div class="grey-box">
GET /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>/subscribers
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.
* **Message ID**: The id of the message to retrieve status for.

### Response
```js
{
  "subscribers":[
    {
      "retries_delay":60,
      "retries_remaining":2,
      "status_code":200,
      "status":"deleted",
      "url":"http://mysterious-brook-1807.herokuapp.com/ironmq_push_2",
      "id":"5831237764476661217"
    },
    {
      "retries_delay":60,
      "retries_remaining":2,
      "status_code":200,
      "status":"deleted",
      "url":"http://mysterious-brook-1807.herokuapp.com/ironmq_push_1",
      "id":"5831237764476661218"
    }
  ]
}
```

## <a name="acknowledge"></a> Acknowledge / Delete Push Message for a Subscriber

This is only for use with long running processes that have previously returned a 202. Read Push Queues page for more information on [Long Running Processes](http://dev.iron.io/mq/reference/push_queues/#how_the_endpoint_should_handle_push_messages)

<div class="grey-box">
DELETE /projects/<span class="variable project_id">{Project ID}</span>/queues/<span class="variable queue_name">{Queue Name}</span>/messages/<span class="variable message_id">{Message ID}</span>/subscribers/<span class="variable subscriber_id">{Subscriber ID}</span>
</div>

#### URL Parameters

* **Project ID**: The project these messages belong to.
* **Queue Name**: The name of queue.
* **Message ID**: The id of the message.
* **Subscriber ID**: The id of the subscriber to delete.

### Response
```js
{
  "msg": "Deleted"
}
```
