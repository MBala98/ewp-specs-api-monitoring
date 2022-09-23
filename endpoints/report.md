Monitoring API Report endpoint
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


### `server_hei_id` (required)

SCHAC ID of the HEI that was the server in the request that is being reported.


### `api_url` (required)

URL of the API that was called in the request that is being reported.


### `client_error_type` (required)

The type of error that is being reported. This value MUST be one of:

  * SERVER_ERROR - When server returned any HTTP code other than 200.
  * INCORRECT_RESPONSE - When the client could not verify the server's response, for example because of some incorrect
    values returned by the server.
  * TIMEOUT - When client decided the server is taking too long to respond. When this option is chosen, parameter
  *connection_timeout* MUST be specified.
  * NETWORK_ERROR - for network errors other than timeout, like DNS failures or SSL certificate errors.


### `http_code` (optional)

The HTTP status code that was returned by the server in the request that is being reported.


### `connection_timeout` (optional)

Time in milliseconds specifying how long client was willing to wait for the response. MUST be specified when the
*client_error_type* has value *TIMEOUT*.


### `message` (optional)

The message containing error description. It's expected that in most cases it will be *developer-message* part
of the response from the server, described [here][dev-msg]. Note that servers do not always
send this value to clients, especially in cases where the return code is different from *4xx*. However, in cases they do
send it, it's recommended to always pass this value to EWP Stats Portal.


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