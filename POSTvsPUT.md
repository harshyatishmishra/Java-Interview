Idempotent - PUT/DELETE/GET
Non-Idempotent - POST

POST- Create Resource
PUT- Update resource

`POST is neither safe nor idempotent.` It is therefore recommended for non-idempotent resource requests.
#### Making two identical POST requests will most-likely result in two resources containing the same information.


`PUT is not a safe operation`, in that it modifies (or creates) state on the server, but `PUT is idempotent`. In other words, if you create or update a resource using 
####  PUT and then make that same call again, the resource is still there and still has the same state as it did with the first call.

If, for instance, calling PUT on a resource increments a counter within the resource, the call is no longer idempotent. Sometimes that happens and it may be enough to document that the call is not idempotent. However, it's recommended to keep PUT requests idempotent. It is strongly recommended to use POST for non-idempotent requests.
