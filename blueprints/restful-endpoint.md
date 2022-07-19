# RESTful endpoint

This blueprint contains guidance around the creation of a RESTful endpoint. 

Much of this has been repurposed from - 

- [Best practices for REST API design](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/#h-use-nouns-instead-of-verbs-in-endpoint-paths)
- [FHIR REST API guidance](https://hl7.org/fhir/http.html)

### Contents
- [Check what exists already](#Check-what-exists-already)
- [Accept and respond with JSON](#accept-and-respond-with-json)
- [Use nouns instead of verbs in endpoint paths](#use-nouns-instead-of-verbs-in-endpoint-paths)
- [Use logical nesting on endpoints](#use-logical-nesting-on-endpoints)
- [Handle errors gracefully and return standard error codes](#handle-errors-gracefully-and-return-standard-error-codes)
- [Handle success consistently](#handle-success-consistently)
- [Allow pagination and sorting](#allow-pagination-and-sorting)
- [Maintain good security practices](#maintain-good-security-practices)
- [Cache data to improve performance](#cache-data-to-improve-performance)
- [Managing Resource Contention](#managing-resource-contention)
- [Versioning](#versioning)

## Check what exists already
Do not create a new endpoint for a container without first checking that you cannot reuse (or modify) an existing one. See the commands catalogue for the container you are working within (https://github.com/UKHomeOffice/callisto-[container]-restapi/docs)

## Accept and respond with JSON
To make sure that when our REST API app responds with JSON that clients interpret it as such, we should set Content-Type in the response header to application/json after the request is made.

## Use nouns instead of verbs in endpoint paths
Nouns represent the entity that the endpoint that we’re retrieving or manipulating. The verb is conveyed by the HTTP method. Common methods include GET, POST, PUT, and DELETE.

- GET retrieves resources.
- POST submits new data to the server.
- PUT updates existing data.
- DELETE removes data.

The verbs map to CRUD operations.

**TODO** - add some guidance around PUT vs PATCH (JSON Patch - https://datatracker.ietf.org/doc/html/rfc6902)

## Use logical nesting on endpoints
When designing endpoints, it makes sense to group those that contain associated information. That is, if one object can contain another object, you should design the endpoint to reflect that. 

For example, if we want an endpoint to get the comments for a news article, we should append the /comments path to the end of the /articles path. 

In the code above, we can use the GET method on the path '/articles/:articleId/comments'. We get comments on the article identified by articleId and then return it in the response. We add 'comments' after the '/articles/:articleId' path segment to indicate that it’s a child resource of /articles.

This makes sense since comments are the children objects of the articles, assuming each article has its own comments. Otherwise, it’s confusing to the user since this structure is generally accepted to be for accessing child objects. The same principle also applies to the POST, PUT, and DELETE endpoints. They can all use the same kind of nesting structure for the path names.

However, nesting can go too far. After about the second or third level, nested endpoints can get unwieldy. Consider, instead, returning the URL to those resources instead, especially if that data is not necessarily contained within the top level object.

For example, suppose you wanted to return the author of particular comments. You could use /articles/:articleId/comments/:commentId/author. But that’s getting out of hand. Instead, return the URI for that particular user within the JSON response instead:

"author": "/users/:userId"

## Handle errors gracefully and return standard error codes
To eliminate confusion for API users when an error occurs, we should handle errors gracefully and return HTTP response codes that indicate what kind of error occurred. This gives maintainers of the API enough information to understand the problem that’s occurred. We don’t want errors to bring down our system, so we can leave them unhandled, which means that the API consumer has to handle them.

Common error HTTP status codes include:

- 400 Bad Request – This means that client-side input fails validation.
- 401 Unauthorized – This means the user isn’t not authorized to access a resource. It usually returns when the user isn’t authenticated.
- 403 Forbidden – This means the user is authenticated, but it’s not allowed to access a resource.
- 404 Not Found – This indicates that a resource is not found.
- 412 Precondition Failed - Usually returned in response to a an attempt to update a resource using stale data
- 500 Internal server error – This is a generic server error. It probably shouldn’t be thrown explicitly.
- 502 Bad Gateway – This indicates an invalid response from an upstream server.
- 503 Service Unavailable – This indicates that something unexpected happened on server side (It can be anything like server overload, some parts of the system failed, etc.).

Error codes need to have messages accompanied with them so that the maintainers have enough information to troubleshoot the issue, but attackers can’t use the error content to carry our attacks like stealing information or bringing down the system.

Whenever our API does not successfully complete, we should fail gracefully by sending an error with information to help users make corrective action.

## Handle success consistently
HTTP success response codes should be used consistently. In most cases a 200 response code will suffice.

- 200 OK - This means that the request has been successful
- 201 Created - Used in conjunction with a POST and indicates that the resource has been created

## Allow pagination and sorting
The databases behind a REST API can get very large. Sometimes, there’s so much data that it shouldn’t be returned all at once because it’s way too slow or will bring down our systems. Therefore, we need ways to control the number of resources that are returned in each response.We don’t want to tie up resources for too long by trying to get all the requested data at once.

Pagination increases performance by reducing the usage of server resources. As more data accumulates in the database, the more important these features become.

We can also specify the fields to sort by in the query string. For instance, we can get the parameter from a query string with the fields we want to sort the data for. Then we can sort them by those individual fields. 

For instance, we may want to extract the query string from a URL like:

http://example.com/articles?sort=+author,-datepublished

Where + means ascending and - means descending. So we sort by author’s name in alphabetical order and datepublished from most recent to least recent.

**TODO** - add some guidance around filtering

## Maintain good security practices
Most communication between client and server should be private since we often send and receive private information. Therefore, using TLS for security is a must (version 1.3).

People shouldn’t be able to access more information that they requested. For example, a normal user shouldn’t be able to access information of another user. They also shouldn’t be able to access data of admins.

To enforce the principle of least privilege, we need to add role checks (for more information see (authorisation)[./authorisation.md]

## Cache data to improve performance
**TODO** - more thought needed (Cache-Control HTTP request header)


## Managing Resource Contention 
Lost Updates , where two clients update the same resource, and the second overwrites the updates of the first, can be prevented using a combination of the ETag and If-Match headers. This is also known as 'Optimistic Locking'.

To support this usage, servers SHOULD always return an ETag and Last-Modified header with each resource:

`HTTP 200 OK
Date: Thurs, 18 Jul 2022 16:09:50 GMT
Last-Modified: Weds, 17 July 2022 12:02:47 GMT
ETag: W/"23"
Content-Type: application/json`

The value of the ETag matches the value of the version id for the resource. Servers are allowed to generate the version id in whatever fashion that they wish, so long as they are unique within the address space of all versions of the same resource. 

If the client wishes to request a version aware update, it submits the request with an If-Match header that quotes the ETag from the server:

`PUT articles/:articleId HTTP/1.1
If-Match: W/"23"`

If the version id given in the If-Match header does not match, the server returns a 412 Precondition Failed status code instead of updating the resource.

Servers can require that clients provide an If-Match header by returning 400 Client Error status codes when no If-Match header is found. 

## Versioning
**TODO**
