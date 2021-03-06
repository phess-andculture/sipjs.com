---
title: SIP.UA | SIP.js
---

# SIP.UA

A user agent (or UA) is associated with a SIP user address and acts on behalf of that user to send and receive SIP requests.  A user agent can register to receive incoming requests, as well as create and send outbound messages.  The user agent also maintains the WebSocket over which its signaling travels.

* TOC
{:toc}

## Construction

### `new SIP.UA([configuration])`

A new user agent is created via the `SIP.UA` constructor.  There are no mandatory parameters for creating a new user agent, although most applications will define at least [`uri`](/api/devel/ua_configuration_parameters/#uri) and [`ws_servers`](/api/devel/ua_configuration_parameters/#ws_servers). Check the full list for optional [UA Configuration Parameters](/api/devel/ua_configuration_parameters/).

### Examples

~~~ javascript
// Create a user agent named Bob, connect, and register to receive invitations.
var bob = new SIP.UA({
  uri: 'bob@example.com',
  ws_servers: ['wss://sip-ws.example.com'],
  register: true
});
bob.start();
~~~

## Instance Methods

### `start()`

Connect to the configured WebSocket server, and restore the previous state if previously stopped. The first time `start()` is called, the UA will also attempt to register if the [`register`](/api/devel/ua_configuration_parameters/#register) parameter in the UA's configuration is set to `true`.

### `stop()`

Saves the current registration state and disconnects from the WebSocket server after gracefully unregistering and terminating any active sessions.

### `register([options])`

Register the UA to receive incoming requests.  Upon successful registration, the UA will emit a `registered` event.

#### Parameters

Name | Type | Description
-----|------|-------------
`options`|`Object`|Optional `Object` with extra parameters (see below)
`options.extraHeaders`|`Array` of `Strings`|Optional `Array` of `Strings` with extra SIP headers for each REGISTER request.

#### Returns

Type    | Description
--------|----------------
`SIP.UA`| This user agent

#### Example

~~~ javascript
var options = {
  'extraHeaders': [ 'X-Foo: foo', 'X-Bar: bar' ]
};

myUA.register(options);
~~~

### `unregister([options])`

Unregisters the UA.

#### Parameters

Name | Type | Description 
-----|------|-------------
`options`|`Object`|Optional `Object` with extra parameters (see below).
`options.all`|`Boolean`|Optional `Boolean` for unregistering all bindings of the same SIP user. Default value is `false`.
`options.extraHeaders`|`Array` of `Strings`|Optional `Array` of `Strings` with extra SIP headers for each REGISTER request.

#### Returns

Type | Description
-----|-------------
`SIP.UA`| This user agent

#### Example

~~~ javascript
var options = {
  'all': true,
  'extraHeaders': [ 'X-Foo: foo', 'X-Bar: bar' ]
};

myUA.unregister(options);
~~~

### `isRegistered()`

#### Returns

Type     | Description
---------|-------------
`Boolean`| `true` if the UA is registered, `false` otherwise

### `isConnected()`

#### Returns

Type     | Description
---------|-------------
`Boolean`| `true` if the WebSocket connection is established, `false` otherwise.







### `message(target, body[, options])`

Sends an instant message making use of SIP MESSAGE method.

#### Parameters

Name | Type | Description 
-----|------|--------------
`target`|`String|`[`SIP.URI`](/api/devel/uri/)|Destination of the call. `String` representing a destination username or a complete SIP URI, or a [`SIP.URI`](/api/devel/uri/) instance.
`body`|`String`|Message content. `String` representing the body of the message.
`options`|`Object`|Optional `Object` with extra parameters (see below).
`options.contentType`|`String`|Optional `String` representing the content-type of the body. Default is `text/plain`.
`options.extraHeaders`|`Array` of `Strings`|Optional `Array` of `Strings` with extra SIP headers for the MESSAGE request.

#### Returns

*The return value of this method implements multiple interfaces.*

Types | Description
------|-------------
[`SIP.Message`](/api/devel/message/), [`SIP.ClientContext`](/api/devel/context/client/) | The newly created MESSAGE. The new Message object implements the shared ClientContext interface for outbound requests.

#### Example

~~~ javascript
var message = myUA.message('bob@example.com', 'Hello, Bob!');
console.log(message.body); // 'Hello, Bob!'
~~~


### `invite(target[, options])`

Invite the target to start a multimedia session.

#### Parameters

|Name                      | Type        | Description |
|--------------------------|-------------|-------------|
|`target`                  |`String|`[`SIP.URI`](/api/devel/uri/)     |Destination of the call. `String` representing a destination username or a complete SIP URI, or a [`SIP.URI`](/api/devel/uri/) instance.|
|`options`                 |`Object`     |Optional `Object` with extra parameters (see below).|
|`options.mediaConstraints`|`Object`     |`Object` with two valid fields (`audio` and `video`) indicating whether the session is intended to use audio and/or video and the constraints to be used. If media constraints are not provided, `{audio: true, video: true}` will be used.|
|`options.mediaStream`     |`MediaStream`|`MediaStream` to transmit to the other end.|
|`options.RTCConstraints`  |`Object`     |`Object` representing RTCPeerconnection constraints|
|`options.extraHeaders`    |`Array` of `Strings` |Optional `Array` of `Strings` with extra SIP headers for the INVITE request.|
|`options.anonymous`       |`Boolean`    |`Boolean` field indicating whether the call should be done anonymously. Default value is `false`.|

#### Returns

*The return value of this method implements multiple interfaces.*

Types | Description
------|-------------
[`SIP.Session`](/api/devel/session/), [`SIP.ClientContext`](/api/devel/context/client/)| The session the target is invited to.  The new Session object implements the shared ClientContext interface for outbound requests. The Session is in a provisional or early state until accepted by the remote target.  Please refer to the Session documentation for more information.

#### Example

~~~ javascript
TBD
~~~

### `request(method, target[, options])`

Send a SIP message.

#### Parameters

Name | Type | Description 
-----|------|--------------
`method`|`String`|The SIP request method to send, e.g. `'INVITE'` or `'OPTIONS'`
`target`|`String|`[`SIP.URI`](/api/devel/uri/)|Destination address. `String` representing a destination username or complete SIP URI, or a [`SIP.URI`](/api/devel/uri/) instance.
`body`|`String`|Message content. `String` representing the body of the message.
`options`|`Object`|Optional `Object` with extra parameters (see below).
`options.body`|`String`|Optional `String` to be included as the body of the request
`options.extraHeaders`|`Array` of `Strings`|Optional `Array` of `Strings` with extra SIP headers for the MESSAGE request.

#### Returns

Type | Description
-----|-------------
[`SIP.ClientContext`](/api/devel/context/client/)| The context surrounding the new outbound request.













## Events

User agent objects extend the [SIP.EventEmitter](/api/devel/eventEmitter/) interface.  Each event emitted by the UA passes specific relevant arguments to its callbacks.

### `connected`

Fired when the WebSocket connection is established.

#### `on('connected', function () {})`

*There are no documented arguments for this event*

### `disconnected`

Fired when the WebSocket connection attempt (or automatic re-attempt) fails.

#### `on('disconnected', function () {})`

*There are no documented arguments for this event*

### `registered`

Fired for a successful registration.

#### `on('registered', function () {})`

*There are no documented arguments for this event*

### `unregistered`

Fired for an unregistration. This event is fired in the following scenarios:

* As a result of an unregistration request, `UA.unregister()`.
* If being registered, a periodic re-registration fails.

#### `on('unregistered', function (cause) {})`

Name | Type | Description 
-----|------|--------------
`cause`||`null` for positive response to un-REGISTER SIP request. If a reregistration fails, this is one value of [Failure and End Causes](/api/devel/causes)


### `registrationFailed`

Fired for a registration failure.

#### `on('registrationFailed', function (cause) {})`

Name | Type | Description 
-----|------|--------------
`cause`||One value of [Failure and End Causes](/api/devel/causes)


### `invite`

Fired when an incoming INVITE request is received.

#### `on('invite', function (session) {})`

*The argument passed to this event implements multiple interfaces.*

Name | Types | Description 
-----|-------|-------------
`session`|[`SIP.Session`](/api/devel/message/), [`SIP.ServerContext`](/api/devel/context/server/)| The inbound session the user agent was invited to. This argument also implements the shared [`SIP.ServerContext`](/api/devel/context/server/) behavior for inbound requests.

### `message`

Fired when an incoming MESSAGE request is received.

#### `on('message', function (message) {})`

*The argument passed to this event implements multiple interfaces.*

Name | Types | Description 
-----|-------|-------------
`message`|[`SIP.Message`](/api/devel/message/), [`SIP.ServerContext`](/api/devel/context/server/)| The inbound message received. This argument also implements the shared [`SIP.ServerContext`](/api/devel/context/server/) behavior for inbound requests.
