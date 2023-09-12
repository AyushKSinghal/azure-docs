---
title: Maintenance Configurations as an Event Grid source
description: This article describes how to use Azure Maintenance Configurations as an Event Grid event source. It provides the schema and links to how-to articles. 
ms.topic: conceptual
author: aysingha
ms.author: aysingha
ms.date: 10/01/2023
---

# Azure Maintenance Configurations as an Event Grid source

This article provides the properties and schema for [Azure Maintenance Configurations](../api-management/index.yml) events. For an introduction to event schemas, see [Azure Event Grid event schema](./event-schema.md). It also gives you links to articles to use API Management as an event source.

## Available event types

API Management emits the following event types:

| Event type | Description |
| ---------- | ----------- |
| Microsoft.Maintenance.PreMaintenanceEvent | Raised before maintenance start and give user opportunity to perform pre maintenance operations. |
| Microsoft.Maintenance.PostMaintenanceEvent | Raised after maintenance completes and give opportunity to perform post maintenance operations. |


## Example event

# [Event Grid event schema](#tab/event-grid-event-schema)
The following example shows the schema of a product created event. The schema of other API Management resource created events is similar.

```json
[{
  "id": "92c502f2-a966-42a7-a428-d3b319844544",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/products/myproduct",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/products/myproduct"
  },
  "eventType": "Microsoft.ApiManagement.ProductCreated",
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2021-07-02T00:47:47.8536532Z"
}]
```

# [Cloud event schema](#tab/cloud-event-schema)

The following example shows the schema of a product created event. The schema of other API Management resource created events is similar. 

```json
[{
  "id": "81dac958-49cf-487e-8805-d0baf0ee485a",
  "source": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/products/myproduct",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/products/myproduct"
  },
  "Type": "Microsoft.ApiManagement.ProductCreated",
  "Time": "2021-07-02T00:38:44.3978295Z",
  "specversion":"1.0"
}]
```
---

# [Event Grid event schema](#tab/event-grid-event-schema)
The following example shows the schema of a user deleted event. The schema of other API Management resource deleted events is similar.

```json
[{
  "id": "92c502f2-a966-42a7-a428-d3b319844544",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/users/apimuser-contoso-com",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/users/apimuser-contoso-com"
  },
  "eventType": "Microsoft.ApiManagement.UserDeleted",
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2021-07-02T00:47:47.8536532Z"
}]
```

# [Cloud event schema](#tab/cloud-event-schema)

The following example shows the schema of a user deleted event. The schema of other API Management resource deleted events is similar. 

```json
[{
  "id": "81dac958-49cf-487e-8805-d0baf0ee485a",
  "source": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/users/apimuser-contoso-com",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/users/apimuser-contoso-com"
  },
  "Type": "Microsoft.ApiManagement.UserDeleted",
  "Time": "2021-07-02T00:38:44.3978295Z",
  "specversion":"1.0"
}]
```
---


## Event properties

# [Event Grid event schema](#tab/event-grid-event-schema)

An event has the following top-level data:

| Property | Type | Description |
| -------- | ---- | ----------- |
| `topic` | string | Full resource path to the event source. This field isn't writeable. Event Grid provides this value. |
| `subject` | string | Publisher-defined path to the event subject. |
| `eventType` | string | One of the registered event types for this event source. |
| `eventTime` | string | The time the event is generated based on the provider's UTC time. |
| `id` | string | Unique identifier for the event. |
| `data` | object | Maintenance Configurations event data. |
| `dataVersion` | string | The schema version of the data object. The publisher defines the schema version. |
| `metadataVersion` | string | The schema version of the event metadata. Event Grid defines the schema of the top-level properties. Event Grid provides this value. |

# [Cloud event schema](#tab/cloud-event-schema)

An event has the following top-level data:

| Property | Type | Description |
| -------- | ---- | ----------- |
| `source` | string | Full resource path to the event source. This field isn't writeable. Event Grid provides this value. |
| `subject` | string | Publisher-defined path to the event subject. |
| `type` | string | One of the registered event types for this event source. |
| `time` | string | The time the event is generated based on the provider's UTC time. |
| `id` | string | Unique identifier for the event. |
| `data` | object | Maintenance Configurations event data. |
| `specversion` | string | CloudEvents schema specification version. |

---

The data object has the following properties:

| Property | Type | Description |
| -------- | ---- | ----------- |
| `resourceUri` | string | The fully qualified ID of the resource that the compliance state change is for. Uses the format, `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup>/Microsoft.ApiManagement/service/<ServiceName>/<ResourceType>/<ResourceName>` |
| `resourceUri` | string | The fully qualifi |
| `resourceUri` | string | The fully qualifi |
| `resourceUri` | string | The fully qualifi |

## Tutorials and how-tos

|Title  |Description  |
|---------|---------|
| [Send events from API Management to Event Grid](../api-management/how-to-event-grid.md)| How to subscribe to API Management events using Event Grid. |

## Next steps

- For an introduction to Azure Event Grid, see [What is Event Grid?](./overview.md)
- For more information about creating an Azure Event Grid subscription, see [Event Grid subscription schema](./subscription-creation-schema.md).

