# referenceAttachment resource type

A link to a file (such as a text file or Word document) on your OneDrive for Business cloud drive and other supported storage locations, attached to a message or event. The  **ContentBytes** property contains the base64-encoded contents of the file.

Derived from [attachment](attachment.md). 

### Methods

| Method       | Return Type  |Description|
|:---------------|:--------|:----------|
|[Get referenceAttachment](../api/referenceattachment_get.md) | [referenceAttachment](referenceattachment.md) |Read properties and relationships of referenceAttachment object.|
|[Delete](../api/attachment_delete.md) | None |Delete referenceAttachment object. |


### Properties
| Property	   | Type	|Description|
|:---------------|:--------|:----------|
|contentType|String|The content type of the attachment.|
|id|String|The attachment ID.  Read-only.|
|isInline|Boolean|Set to true if this is an inline attachment.|
|lastModifiedDateTime|DateTimeOffset|The date and time when the attachment was last modified. The Timestamp type represents date and time information using ISO 8601 format and is always in UTC time. For example, midnight UTC on Jan 1, 2014 would look like this: `'2014-01-01T00:00:00Z'`|
|name|String|The name representing the text that is displayed below the icon representing the embedded attachment. This does not need to be the actual file name.|
|size|Int32|The size in bytes of the attachment.|


### Relationships
None



### JSON representation

Here is a JSON representation of the resource

<!-- {
  "blockType": "resource",
  "optionalProperties": [

  ],
  "@odata.type": "microsoft.graph.referenceattachment"
}-->

```json
{
  "contentType": "string",
  "id": "string (identifier)",
  "isInline": true,
  "lastModifiedDateTime": "String (timestamp)",
  "name": "string",
  "size": 1024
}

```

<!-- uuid: 8fcb5dbc-d5aa-4681-8e31-b001d5168d79
2015-10-25 14:57:30 UTC -->
<!-- {
  "type": "#page.annotation",
  "description": "referenceAttachment resource",
  "keywords": "",
  "section": "documentation",
  "tocPath": ""
}-->