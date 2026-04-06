
## PUT vs. POST vs. PATCH
PUT and POST are very common http methods, the key difference is that PUT is used to update specified existing resource while POST is used to create new resource.
There is also the method PATCH which is another way to update a particular resource. The difference between PUT and PATCH is that PATCH alters only the data that is specified whereas PUT replaces the entire resource with new resource comprised the given data.

## What is the REST Message

## Core Components of a HTTP Request
- Method (verb)
- URI
- HTTP Version
- Request Header
- Request Body
## Core Components of a HTTP Response
- Status Code
- HTTP Version
- Response Header
- Response Body
## What is an idempotent method and why they are important?
Idempotent method is the one that yields the same response, regardless of how many times a request is sent.

REST APIs are designed on the principle that the client should depend on previous API calls, hence why the REST architecture is made to be stateless. Therefore REST APIs should be made such that their methods are idempotent when possible.

## What is difference between Idempotent and Safe HTTP methods?
An idempotent method is one that can be called multiple times without changing responses while safe method is one that does not change a resource meaning it may read but not write.