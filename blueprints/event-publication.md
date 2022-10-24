# Event publication

Events in Callisto correspond to changes that happen to resources that have been exposed via the various RESTful endpoints.

An event wraps the effected Resource in an envelope of sorts. The envelope decorates the Resource that it carries with metadata about the event. This metadata describes the lifecycle event that happened to the Resource that is being wrapped.

## Resource lifecycle
Callisto event producers are in charge of publishing what they see as significant events in the life of Resources that they expose. There are a number of lifecycle events that can happen to a Resource but a given publisher may not choose to broadcast all of them (see the documentation for a specific publisher). The complete set of lifecycle events is listed below - 

- create - to be used when a new Resource has been created
- update - to be used when an existing Resource has been modified (this includes changes to contained child Resources where that change might be a deletion of a given child Resource)
- delete - to be used when an existing Resource has been removed. No further updates to that Resource are expected (**TODO** - what about undo?)

## Register Resource schema
**TODO** - in order to support deserialisation and validation of the `resource.content` the consumer needs to know what the schema of the content is. Each container shuld publish schema (in Avro) - can we have Avro solve this for us without having to invent a bespoke approach? 


## Event model

Note that the event is flexible in terms of the way it can carry the effected Resource. The `resource.content` property is an untyped object and is therefore free to carry any content. Due to this flexibility a publisher could choose to wrap a ResourceReference (see definitions/ResourceReference in the schema below) instance instead of the full Resource. Producers might do this when a Resource has been deleted or if they wish to save bandwidth

```yaml
{
  "$id": "https://callisto.digital.homeoffice.gov.uk/event.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Event",
  "description": "Describes an action and the Callisto resource that it was performed against",
  "type": "object",
  "properties": {
    "action" : {
    	"$ref": "#/definitions/Action"
    }, 
    "resource" : {
    	"type": "object"
		"properties": {
			"schema": {
				"description": "The schema that the content conforms to. Used by consumers for deserialisation and validation",
				"type": "string",
				"format": "uri"
			}
			"content": {
				"description": "The actual Resource that is the subject of the event",
				"type": "object"
			}
		},
		"required": ["schema", "content"]
    }
  },
  "required": ["action", "resource"],
  "definitions":{
    "ResourceReference": {
      	 "description": "A reference to a Resource. Can be used to retrieved full Resource",
         "type": "object",
         "properties": {
         	"tenantId": {
            	"type": "number"
            },
            "id": {
            	"type": "number"
            },
             "version": {
            	"type": "number"
            } 
         },
         "required": ["tenantId", "id", "version"]
    },
    "Action": {
      "description": "The kind of operation that was performed on the resource",
      "type": "string",
	  "enum": ["create", "update", "delete"]
	  }
  }
}
```
source - [`event-schema.json`](../schema/event-schema.json)

## Out of scope
- Retrieval of Resource from the ResourceReference eg a URL for the resource and a URL to the schema that the Resource conforms to

## Further reading
- https://martinfowler.com/articles/201701-event-driven.html
- https://blog.frankdejonge.nl/the-different-types-of-events-in-event-driven-systems/
