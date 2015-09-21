# API Design Cheat Sheet
1. Build the API with consumers in mind--as a product in its own right.
    * Not for a specific UI.
    * Embrace flexibility / tunability of each endpoint (see #5, 6 & 7).
    * Eat your own dogfood, even if you have to mockup an example UI.

1. Use the Collection Metaphor.
    * Two URLs (endpoints) per resource:
        * The resource collection (e.g. /orders)
        * Individual resource within the collection (e.g. /orders/{orderId}).
    * Use plural forms (‘orders’ instead of ‘order’).
    * Alternate resource names with IDs as URL nodes (e.g. /resources/{id}/resources/{id})
    * Keep URLs as short as possible. Preferably, no more-than three nodes per URL.

1. Use nouns as resource names (e.g. don’t use verbs in URLs).

1. Make resource representations meaningful.
    * “No Naked IDs!” No plain IDs embedded in responses. Use links and reference objects.
    * Design resource representations. Don’t simply represent database tables.
    * Merge representations. Don’t expose relationship tables as two IDs-represent meaningful resources.

1. Support filtering, sorting, and pagination on collections.

1. Support link expansion of relationships. Allow clients to expand the data contained in the response by including additional representations instead of links.

1. Support field projections on resources. Allow clients to reduce the number of fields that come back.

1. Use the HTTP method names to mean something:
    * POST - create and other non-idempotent operations.
    * PUT - update.
    * GET - read a resource or collection.
    * DELETE - remove a resource or collection.

1. Use HTTP status codes to be meaningful.
    * 200 - Success.
    * 201 - Created. Returned on successful creation of a new resource. Include a 'Location' header with a link to the newly-created resource.
    * 400 - Bad request. Data issues such as invalid JSON, etc.
    * 401 - User could not be authenticated
    * 402 - User authenticated but unauthorized to perform the operation.
    * 404 - Not found. Resource not found on GET.
    * 409 - Conflict. Duplicate data or invalid data state would occur.

1. Use ISO 8601 timepoint formats for dates in representations.

1. Consider connectedness.
    * Utilize a linking strategy. Some popular examples are:
        * [HAL](http://stateless.co/hal_specification.html)
        * [Siren](https://github.com/kevinswiber/siren)
        * [JSON-LD](http://json-ld.org/)
        * [Collection+JSON](http://amundsen.com/media-types/collection/)

1. Use [OAuth2](http://oauth.net/2/) to secure your API.
    * Use HTTPS / TLS to access your API.
    * Use a Bearer token for authentication.

1. Evolution over versioning. However, if versioning, use Accept header instead of URL.  The API should always accept 'application/json' as the default in a non-breaking manner, with adding new properties being the most common non-breaking contract change.  However, sometimes you might need to have the API support a new version of a resource.  To do this, have your API accept the new version in the Accept header and respond appropriately.  The idea is to make this painful so we don't version unless absolutely necessary, we evolve.
e.g,
Accept : application/helix.enroll.user-v2+json

1. Consider Cache-ability.
    * At a minimum, use the following response headers:
        * ETag - An arbitrary string for the version of a representation. Make sure to include the media type in the hash value, because that makes a different representation. (ex: ETag: "686897696a7c876b7e")
        * Date - Date and time the response was returned (in RFC1123 format). (ex: Date: Sun, 06 Nov 1994 08:49:37 GMT)
        * Cache-Control - The maximum number of seconds (max age) a response can be cached. However, if caching is not supported for the response, then no-cache is the value. (ex: Cache-Control: 360 or Cache-Control: no-cache)
        * Expires - If max age is given, contains the timestamp (in RFC1123 format) for when the response expires, which is the value of Date (e.g. now) plus max age. If caching is not supported for the response, this header is not present. (ex: Expires: Sun, 06 Nov 1994 08:49:37 GMT)
        * Pragma - When Cache-Control is 'no-cache' this header is also set to 'no-cache'. Otherwise, it is not present. (ex: Pragma: no-cache)
        * Last-Modified - The timestamp that the resource itself was modified last (in RFC1123 format). (ex: Last-Modified: Sun, 06 Nov 1994 08:49:37 GMT)
