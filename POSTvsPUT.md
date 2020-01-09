Idempotent - PUT/DELETE/GET

Non-Idempotent - POST

POST- Create Resource
PUT- Update resource

`POST is neither safe nor idempotent.` It is therefore recommended for non-idempotent resource requests.
#### Making two identical POST requests will most-likely result in two resources containing the same information.


`PUT is not a safe operation`, in that it modifies (or creates) state on the server, but `PUT is idempotent`. In other words, if you create or update a resource using 
####  PUT and then make that same call again, the resource is still there and still has the same state as it did with the first call.

If, for instance, calling PUT on a resource increments a counter within the resource, the call is no longer idempotent. Sometimes that happens and it may be enough to document that the call is not idempotent. However, it's recommended to keep PUT requests idempotent. It is strongly recommended to use POST for non-idempotent requests.


####  DELETE operations are idempotent. If you DELETE a resource, it's removed. Repeatedly calling DELETE on that resource ends up the same: the resource is gone.

If calling DELETE say, decrements a counter (within the resource), the DELETE call is no longer idempotent.

There is a caveat about DELETE idempotence, however. Calling DELETE on a resource a second time will often return a 404 (NOT FOUND) since it was already removed and therefore is no longer findable. This, by some opinions, makes DELETE operations no longer idempotent, however, the end-state of the resource is the same. Returning a 404 is acceptable and communicates accurately the status of the call.
