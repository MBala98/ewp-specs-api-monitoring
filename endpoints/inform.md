Monitoring API Inform endpoint
=================================================

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This endpoint allows clients to inform the EWP Stats Portal about any incorrect interactions encountered in the EWP
network.


Request method
--------------

 * Requests MUST be made with HTTP POST method.


Request parameters
------------------

Parameters MUST be provided in the regular `application/x-www-form-urlencoded`
format.


### `client_hei_id` (required)

SCHAC ID of the HEI that was the client in the server-client interaction that is being reported. Note that this ID is
the ID of this API caller.


### `server_hei_id` (required)

SCHAC ID of the HEI that was the server in the server-client interaction that is being reported.


### `api_name` (required)

Name of the API that was called in the interaction that is being reported. The names must be the same as in the *name* 
element in the manifest file of reported API.


### `http_code` (required)

The HTTP status code that was returned by the server in the interaction that is being reported.


### `error_type` (required)

The type of error that is being reported. This value MUST be one of:

  * CLIENT_ERROR - When server returned *4xx* HTTP status code.
  * SERVER_ERROR - When server returned *5xx* HTTP status code.
  * TIMEOUT - When client decided the server is taking too long to respond.
  * INCORRECT_PARAMETERS - When client sees some incorrect values of parameters in the server's response.
  * MISSING_PARAMETERS - When client sees some missing parameters in the server's response.
  * OTHER - for all cases not included above.


### `dev_msg` (optional)

The *developer-message* part of the response from the server, described [here][dev-msg]. Note that servers do not always
send this value to clients, especially in cases where the return code is different from *4xx*. However, in cases they do
send it, it's recommended to always pass this value to EWP Stats Portal.


### `incorrect_params` (repeatable, optional)

A list of names of parameters that had incorrect values in the server's response. Note that this parameter should be 
combined with the *error_type* of INCORRECT_PARAMETERS.


### `missing_params` (repeatable, optional)

A list of names of missing parameters that client expected to see in the response, but did not. Note that this parameter should be
combined with the *error_type* of MISSING_PARAMETERS


### `extra_info` (repeatable, optional)

Any additional information that you think does not fit above parameters but may still be useful.

Permissions
-----------

Every EWP network member can call this endpoint. The authorization method is described in the [API description](../README.md).


Handling of invalid parameters
------------------------------

 * General [error handling rules][error-handling] apply.



[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[error-handling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#error-handling
[dev-msg]: https://github.com/erasmus-without-paper/ewp-specs-architecture/blob/stable-v1/common-types.xsd#L400