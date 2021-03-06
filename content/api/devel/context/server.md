---
title: SIP.ServerContext | SIP.js
---
# SIP.ServerContext

A `SIP.ServerContext` encapsulates the behavior required to receive and send replies to a request.  It is typically mixed in with behavior from a method-specific class, such as [`SIP.Session`](/api/devel/session/) or [`SIP.Message`](/api/devel/message/).

* TOC
{:toc}

<!--
## Construction

Typically, construction of a ServerContext is managed by a `SIP.UA` and happens automatically upon receipt of a new incoming request. However, advanced users may construct ServerContexts manually.

### `new SIP.ServerContext(ua, request)`

#### Parameters

Name | Type | Description
-|-|-
ua | `SIP.UA` | The user agent that received the request.
request | `SIP.IncomingRequest` | The request received.

-->

## Instance Attributes

### `ua`

[`SIP.UA`](/api/devel/ua/) - The user agent which received the request.

### `method`

`String` - The SIP method of the request. For example, `"INVITE"` or `"MESSAGE"`.

### `request`

[`SIP.IncomingRequest`](/api/devel/incomingMessage/) - The request received.

### `localIdentity`

[`SIP.NameAddrHeader`](/api/devel/nameAddrHeader/) - The To header field value, representing the local endpoint. This is typically the URI of the UA as a `SIP.NameAddrHeader`.

### `remoteIdentity`

[`SIP.NameAddrHeader`](/api/devel/nameAddrHeader/) - The From header field value, representing the remote endpoint.

### `data`

`Object` - An empty object.  Define custom application data here. *Note: SIP.js may overwrite any custom attributes defined outside of the data object.*



## Instance Methods

### `progress([options])`

Send a provisional (100-199) response. By default, `progress` will send a `180 Ringing` response with no body.

#### Parameters

Name | Type | Description
-|-|-
`options`|`Object`|Optional `Object` with extra parameters (see below).
`options.statusCode`|`Integer`|Optional SIP response code between 100 and 199 to be used in the reply. By default, 180 will be used.
`options.reasonPhrase`|`String`|Optional SIP description of the response code. If not specified, the reason phrase for the response code from SIP.C.REASON_PHRASE will be used, or the empty string.
`options.extraHeaders`|`Array` of `String`|Optional list of extra headers to be added to the response.
`options.body`|`String`|Optional body to include with the response.

#### Returns

Type | Description
-|-
`SIP.ServerContext`|This server context.

### `accept([options])`

Send a successful (200-299) response. By default, `accept` will send a `200 OK` response with no body.

#### Parameters

Name | Type | Description
-|-|-
`options`|`Object`|Optional `Object` with extra parameters (see below).
`options.statusCode`|`Integer`|Optional SIP response code between 200 and 299 to be used in the reply. By default, 200 will be used.
`options.reasonPhrase`|`String`|Optional SIP description of the response code. If not specified, the reason phrase for the response code from SIP.C.REASON_PHRASE will be used, or the empty string.
`options.extraHeaders`|`Array` of `String`|Optional list of extra headers to be added to the response.
`options.body`|`String`|Optional body to include with the response.

#### Returns

Type | Description
-|-
`SIP.ServerContext`|This server context.

### `reject([options])`

Send a failure (300-699) response. By default, `reject` will send a `480 Temporarily Unavailable` response with no body.

#### Parameters

Name | Type | Description
-|-|-
`options`|`Object`|Optional `Object` with extra parameters (see below).
`options.statusCode`|`Integer`|Optional SIP response code between 300 and 699 to be used in the reply. By default, 480 will be used.
`options.reasonPhrase`|`String`|Optional SIP description of the response code. If not specified, the reason phrase for the response code from SIP.C.REASON_PHRASE will be used, or the empty string.
`options.extraHeaders`|`Array` of `String`|Optional list of extra headers to be added to the response.
`options.body`|`String`|Optional body to include with the response.

#### Returns

Type | Description
-|-
`SIP.ServerContext`|This server context.

### `reply([options])`

Send any (100-699) response. `reply` does not have default settings and will fail without a specified status code.

#### Parameters

Name | Type | Description
-|-|-
`options`|`Object`|`Object` with extra parameters (see below).
`options.statusCode`|`Integer`|SIP response code between 100 and 699 to be used in the reply.
`options.reasonPhrase`|`String`|Optional SIP description of the response code. If not specified, the reason phrase for the response code from SIP.C.REASON_PHRASE will be used, or the empty string.
`options.extraHeaders`|`Array` of `String`|Optional list of extra headers to be added to the response.
`options.body`|`String`|Optional body to include with the response.

#### Returns

Type | Description
-|-
`SIP.ServerContext`|This server context.






## Events

### `progress`

Fired each time a provisional (100-199) response is sent.

#### `on('progress', function (data) {})`

Name | Type | Description
-----|------|------------
`data`|`Object`|A wrapper object containing the event data
`data.code`|`Integer`|The status code of the sent response, between 100 and 199.
`data.response`|[`SIP.IncomingResponse`](/api/devel/incomingResponse)|The sent response

### `accepted`

Fired each time a successful final (200-299) response is sent.

#### `on('accepted', function (data) {})`

Name | Type | Description
-----|------|------------
`data` | `Object` | A wrapper object containing the event data
`data.code` | `Integer` | The status code of the sent response, between 200 and 299.
`data.response` | [`SIP.IncomingResponse`](/api/devel/incomingResponse) | The sent response

### `rejected`

Fired each time an unsuccessful final (300-699) response is sent. *Note: This will also emit a `failed` event.*

#### `on('rejected', function (data) {})`

Name | Type | Description
-----|------|------------
`data` | `Object` | A wrapper object containing the event data
`data.code` | `Integer` | The status code of the sent response, between 300 and 699.
`data.response` | [`SIP.IncomingResponse`](/api/devel/incomingResponse/) | The sent response
`data.cause` | `String` | The reason phrase associated with the SIP response code.

### `failed`

Fired when the request fails, whether due to an unsuccessful final response or due to timeout, transport, or other error.

#### `on('failed', function (data) {})`

Name | Type | Description
-----|------|------------
`data` | `Object` | A wrapper object containing the event data
`data.code` | `Integer` | The status code of the sent response, between 300 and 699, or 0 if the failure was not due to a received response.
`data.response` | [`SIP.IncomingResponse|null`](/api/devel/incomingResponse/) | The sent response, or `null` if the failure was not due to a received response.
`data.cause` | `String` | The reason phrase associated with the SIP response code, or one of `SIP.C.causes` if the failure was not due to a sent response.

