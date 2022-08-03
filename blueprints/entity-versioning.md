# Entity versioning

Callisto entities can have a version identifier that changes if the contents of the entity change.

The version identifier is scoped by the entity i.e. two different instances of the same entity could have the same version. The same version identifier can never be used for more than one version of the same entity.

There is no fixed order for version ids - clients cannot assume that a version identifier that comes after another one either numerically or alphabetically represents a later version.

The same version identifier can never be used for more than one version of the same resource.  

When a container receives a write command (create, update or delete) then it will update the version identifier. 

The version identifier is a key element in the strategy to manage resource contention as detailed on the [RESTful endpoint blueprint](./restful-endpoint.md#managing-resource-contention)

## TBC
:warning: **The content below is under active development and may change without notice** :warning:

### Handling deletions
From a version history respect, deleting an entity is the equivalent of creating a special kind of history entry that has no content and is marked as deleted. 

For example, suppose a `TimeEntry` entity with ID 123 is created and subsequently deleted. This will cause a second version of the `TimeEntry/123` entity to be created with a distinct version identifier that is also marked as deleted.

This `TimeEntry` will no longer appear in search results, and attempts to read the resource will fail (see [Handle errors gracefully and return standard error codes for advice on how to deal with this situation](./restful-endpoint.md#handle-errors-gracefully-and-return-standard-error-codes)).
