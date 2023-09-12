---
title: Maintenance Configurations as an Event Grid source
description: This article describes how to use Azure Maintenance Configurations as an Event Grid event source. It provides the schema and links to how-to articles. 
ms.topic: conceptual
author: aysingha
ms.author: aysingha
ms.date: 10/01/2023
---

# Azure Maintenance Configuration as an Event Grid source

This article provides the properties and schema for [Azure Maintenance Configurations](../virtual-machines/maintenance-configurations.md) events. For an introduction to event schemas, see [Azure Event Grid event schema](./event-schema.md). It also gives you links to articles to use API Management as an event source.

## Available event types

Maintenance Configuration emits the following event types:

| Event type | Description |
| ---------- | ----------- |
| Microsoft.Maintenance.PreMaintenanceEvent | Raised before maintenance start and give user opportunity to perform pre maintenance operations. |
| Microsoft.Maintenance.PostMaintenanceEvent | Raised after maintenance completes and give opportunity to perform post maintenance operations. |

## Example event

# [Event Grid event schema](#tab/event-grid-event-schema)
The following example shows the schema of a Pre Maintenance event: 

```json
[{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration/providers/microsoft.maintenance/applyupdates/20230509150000",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration",
  "subject": "contosomaintenanceconfiguration",
	"data":
	{
	   "correlationId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration/providers/microsoft.maintenance/applyupdates/20230509150000", 
	   "maintenanceConfigurationId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration", 
	   "startDateTime": "2023-05-09T15:00:00Z", 
	   "endDateTime": "2023-05-09T18:55:00Z", 
	   "cancellationCutOffDateTime": "2023-05-09T14:59:00Z", 
	   "resourceSubscriptionIds": ["subscription guid 1", "subscription guid 2"]
	}
	"eventType": "Microsoft.Maintenance.PreMaintenanceEvent",
	"eventTime": "2023-05-09T14:25:00.3717473Z",
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

The schema for a Post Maintenance Event is: 

```json
[{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration/providers/microsoft.maintenance/applyupdates/20230509150000",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration",
  "subject": "contosomaintenanceconfiguration",
	"data":
	{
	   "correlationId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration/providers/microsoft.maintenance/applyupdates/20230509150000", 
	   "maintenanceConfigurationId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration", 
	   "status": "Succeeded",
	   "startDateTime": "2023-05-09T15:00:00Z", 
	   "endDateTime": "2023-05-09T18:55:00Z", 
	   "resourceSubscriptionIds": ["subscription guid 1", "subscription guid 2"]
	}
	"eventType": "Microsoft.Maintenance.PostMaintenanceEvent",
	"eventTime": "2023-05-09T15:55:00.3717473Z",
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```
# [Cloud event schema](#tab/cloud-event-schema)

The following example shows the schema of a Pre Maintenance event: 

```json
[{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration/providers/microsoft.maintenance/applyupdates/20230509150000",
  "source": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration",
  "subject": "contosomaintenanceconfiguration",
	"data":
	{
	   "correlationId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration/providers/microsoft.maintenance/applyupdates/20230509150000", 
	   "maintenanceConfigurationId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration", 
	   "startDateTime": "2023-05-09T15:00:00Z", 
	   "endDateTime": "2023-05-09T18:55:00Z", 
	   "cancellationCutOffDateTime": "2023-05-09T14:59:00Z", 
	   "resourceSubscriptionIds": ["subscription guid 1", "subscription guid 2"]
	}
	"type": "Microsoft.Maintenance.PreMaintenanceEvent",
	"time": "2023-05-09T14:25:00.3717473Z",
  "specversion": "1.0"
}]
```

The schema for a Post Maintenance event event is: 

```json
[{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration/providers/microsoft.maintenance/applyupdates/20230509150000",
  "source": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration",
  "subject": "contosomaintenanceconfiguration",
	"data":
	{
	   "correlationId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration/providers/microsoft.maintenance/applyupdates/20230509150000", 
	   "maintenanceConfigurationId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Maintenance/maintenanceConfigurations/contosomaintenanceconfiguration", 
	   "status": "Succeeded",
	   "startDateTime": "2023-05-09T15:00:00Z", 
	   "endDateTime": "2023-05-09T18:55:00Z", 
	   "resourceSubscriptionIds": ["subscription guid 1", "subscription guid 2"]
	}
	"type": "Microsoft.Maintenance.PostMaintenanceEvent",
	"time": "2023-05-09T15:55:00.3717473Z",
  "specversion": "1.0"
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
| `data` | object | App Configuration event data. |
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
| `data` | object | App Configuration event data. |
| `specversion` | string | CloudEvents schema specification version. |

---

The data object has the following properties:

| Property | Type | Description |
| -------- | ---- | ----------- |
| `correlationId` | string | The resource id of specific maintenance schedule instance. |
| `maintenanceConfigurationId` | string | The resource id of maintenance configuration. |
| `startDateTime` | string | The maintenance schedule start time. |
| `endDateTime` | string | The maintenance schedule end time. |
| `cancellationCutOffDateTime` | string | The maintenance schedule instance cancellation cut-off time. |
| `resourceSubscriptionIds` | string | The subscription ids from which VMs are included in this schedule instance.  |
| `status` | string | The completion status of maintenance schedule instance. |

## Tutorials and how-tos

|Title  |Description  |
|---------|---------|
| [TODO - Send events from Maintenance Configuration to Event Grid](../virtual-machines/maintenance-configurations.md)| How to subscribe to Maintenance Configuration events using Event Grid. |

## Next steps

- For an introduction to Azure Event Grid, see [What is Event Grid?](./overview.md)
- For more information about creating an Azure Event Grid subscription, see [Event Grid subscription schema](./subscription-creation-schema.md).
