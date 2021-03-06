---
title: SIP.uri | SIP.js
---
# SIP.uri

The class SIP.uri represents a SIP URI and provides a set of attributes and methods to access and set different parts of a URI.

* TOC
{:toc}

## Construction

Typically, construction is managed by a `SIP.UA` when a URI is needed. However, advanced users may construct URIs manually.

### new SIP.URI(scheme, user, host, port[, parameters, headers])

#### Parameters

Name | Type | Description
-----|------|--------------
`scheme`|`String`|The scheme used for this URI
`user`|`String`|The user used for this URI
`host`|`String`|The host used for this URI
`port`|`Number`|The port number to use for this URI
`parameters`|`Array of String`|Optional parameters to use for this URI
`headers`|`Array of String`|Optional headers to use for this URI

## Instance Variables

### `scheme`

`String` - Represents the URI scheme (converted to lower case when set)

### `user`

`String` - Represents the URI user

### `host`

`String` - Represents the URI host (converted to lower case when set)

### `port`

`Number` - Represents the URI port

## Instance Methods

### `setParam(key[, value])`

Creates or replaces the given URI parameter with the given value or null if no value is provided

#### Parameters

Name | Type | Description
-----|------|--------------
`key`|`String`|Parameter name
`value`|`String`|Optional parameter value

### `getParam(key)`

Gets the value of the given URI parameter. Returns `undefined` if the parameter does not exist in the parameter set.

#### Parameters

Name | Type | Description
-----|------|--------------
`key`|`String`|Parameter name

#### Returns

Type | Description
-|-
`String`|Value of the given URI parameter.

### `hasParam(key)`

Verifies the existence of the given URI parameter.

#### Parameters

Name | Type | Description
-----|------|--------------
`key`|`String`|Parameter name

#### Returns

Type | Description
-|-
`boolean`|`true` if the parameter exists, `false` otherwise.

### `deleteParam(key)`

Deletes the given parameter from the URI.

#### Parameters

Name | Type | Description
-----|------|--------------
`key`|`String`|Parameter name

#### Returns

Type | Description
-|-
`String`|Value of the deleted URI parameter

### `clearParams()`

Removes all of the URI parameters.

### `setHeader(name, value)`

Creates or replaces the given URI header with the given value.

#### Parameters

Name | Type | Description
-----|------|--------------
`name`|`String`|Header name
`value`|`String`|Header value

### `getHeader(name)`

Gets the value of the given URI header.

#### Parameters

Name | Type | Description
-----|------|--------------
`name`|`String`|Header name

#### Returns

Type | Description
-|-
`Array of String`|Value of the given URI header, or `undefined` if the header does not exist

### `hasHeader(name)`

Verifies the existence of the given URI header.

#### Parameters

Name | Type | Description
-----|------|--------------
`name`|`String`|Header name

#### Returns

Type | Description
-|-
`boolean`|`true` if the header exists, `false` otherwise.

### `deleteHeader(name)`

Deletes the given header from the URI.

#### Parameters

Name | Type | Description
-----|------|--------------
`name`|`String`|Header name

#### Returns

Type | Description
-|-
`Array of String`|Value of the deleted URI header.

### `clearHeaders()`

Removes all the URI headers.

### `clone()`

Returns a cloned `SIP.URI` instance of the URI.

#### Returns

Type | Description
-|-
`SIP.URI`|A clone of the current `SIP.URI`

### `toString()`

Returns a `String` representing the URI. Characters that can’t appear unescaped are escaped as stated in the BNF grammar of the RFC 3261.

## Static Methods

### `parse(uri)`

Parses the given `String` against the SIP URI grammar rule.

#### Parameters

Name | Type | Description
-----|------|--------------
`uri`|`String`|String to parse against the SIP URI grammar rule.

#### Returns

Type | Description
-|-
`SIP.URI`|The parsed `SIP.URI` on success, `undefined` otherwise
