Monitoring API
=========================================

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This document describes the **Monitoring API** implemented by the EWP Stats Portal, which is hosted by EWP administrators. 
This API allows clients in EWP network to inform network's administrators about any issues encountered while interacting
with the servers. This API SHOULD NOT be implemented by anyone other than EWP Stats Portal. Clients SHOULD call this API
in case of detecting any exceptions or incorrect responses from any of the servers in the EWP network. 
Calls to this API MUST NOT contain any private data.


Introduction
----------------------

The distributed design of EWP network, while flexible, makes it hard to control and verify interactions happening
in the network. The errors could arise for a variety of reasons - bugs in implementation, incorrect data, or unreachable
servers. In order to minimize the number of errors in the long term and fix new ones quickly, members of EWP network
should actively report any incorrect behaviours. We decided clients should take up this responsibility, as they are the
ones initializing interactions and they know exactly what they want to receive from the server. 


Endpoints
---------

This API consists of a single endpoint called the [inform endpoint](endpoints/inform.md).
The details of this endpoint can be found on a separate page linked above.


Server implementing this API
----------------

The server implementing this API is called EWP Stats Portal. It can be identified by HEI id listed in [architecture specification][specs-arch].


When to call
------------

The general rule to calling this API is: Call it in every case you do error handling or exception catching. This implies that
every HTTP status code that is *4xx* or *5xx* should be reported, as well as connection timeouts. However, that is not all. 
For example server could return status code 200, but in reality the data returned is lacking, incorrect, or generally
does not make sense. This situation also warrants a call to monitoring server.

Due to the number of different APIs it's not possible to clearly define every potential incorrect interaction in the network.
It's better to send too much rather than too little, but remember to use your common sense.


What happens after a call
-------------------------

EWP Stats Portal gathers and analyzes all the data sent to it through monitoring API. In some cases it may email a provider
implementing a server that is the source of incorrect interaction, with an information about a bug and an offer to help.
It does not mean that every monitoring API call will end with an email to server provider, just in cases deemed fit.


Privacy
-------

EWP administrators are not authorized to access private data that is being exchanged in the network. That is why calls to
monitoring API MUST NOT contain any private data. Clients should restrain to data described in the request parameters.

Security
--------

The server implementing this API uses HTTP Signature for authenticating clients. All clients calling this API
MUST follow the requirements listed in the [HTTP Signature specification][sec-httpsig].



[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[sec-httpsig]: https://github.com/erasmus-without-paper/ewp-specs-sec-cliauth-httpsig
[specs-arch]: https://github.com/erasmus-without-paper/ewp-specs-architecture#permissions