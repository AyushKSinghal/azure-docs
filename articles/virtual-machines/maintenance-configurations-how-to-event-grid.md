---
title: Send events from Maintenance Configuration to Event Grid
description: In this quickstart, you enable Event Grid events for your Azure Maintenance Configuration instance, then send  events to a sample application.
author: aysingha
ms.topic: how-to
ms.service: maintenance-configuration
ms.author: aysingha
ms.date: 10/01/2023
ms.custom: ignite-fall-2021
---

# Send events from Maintenance Configuration to Event Grid

Maintenance Configuration integrates with [Azure Event Grid](../event-grid/overview.md) so that you can send event notifications to other services and trigger downstream processes. Event Grid is a fully managed event routing service that uses a publish-subscribe model. Event Grid has built-in support for Azure services like [Azure Functions](../azure-functions/functions-overview.md) and [Azure Logic Apps](../logic-apps/logic-apps-overview.md), and can deliver event alerts to non-Azure services using webhooks.

For example, using integration with Event Grid, you can build an application that start or shutdown VM, and sends an email notification each time maintenance schedule start or completes.

In this article, you subscribe to Event Grid events for a Maintenance Configuration, trigger events, and send the events to an endpoint that processes the data. To keep it simple, you send events to a sample storage queue that show message.

## Prerequisites
- Create a Storage Account
- Create a  InGuest Scoped Maintenance Configuration. Create it using [Portal](maintenance-configurations-portal.md), [CLI](maintenance-configurations-portal.md), or [PowerShell](maintenance-configurations-portal.md)
- Register the Event Grid resource provider

[!INCLUDE [event-grid-register-provider-portal.md](../../includes/event-grid-register-provider-portal.md)]

## Subscribe to Maintenance Configuration events

In Event Grid, you subscribe to a *topic* to tell it which events you want to track, and where to send them. Here, you create a subscription to events in your Maintenance Configuration instance.

1. In the [Azure portal](https://portal.azure.com), navigate to your Maintenance Configuration instance.
1. Select **Maintenance Events > + Event Subscription**.
    :::image type="content" source="media/maintenance-configurations-how-to-event-grid/eventsblade.jpg" alt-text="Maintenance Configuration Events blade in Azure portal":::
1. On the **Basic** tab:
    * Enter a descriptive **Name** for the event subscription.
    * In **Event Types**, select one or more event types to send to Event Grid. For example, select both **Maintenance Pre Event and Maintenance Post Event** 
    * In **Endpoint Details**, select the **Storage Queue** event type, click **Select an endpoint**, enter your **Storage Account** and create new queue **samplequeue**. Example: `https://myapp.azurewebsites.net/api/updates`.
    * Select **Confirm selection**.
1. Leave the settings on the remaining tabs at their default values, and then select **Create**.

    :::image type="content" source="media/maintenance-configurations-how-to-event-grid/eventsubscription.jpg" alt-text="Create an event subscription in Azure portal":::

## Trigger and view events

You've subscribed to your Maintenance Configuration instance with Event Grid, now change Maintenance Configuration schedule time to current time + 40 mins.

If your event subscription includes the **Maintenance Pre Event** event, upcoming Maintenance Configuration schedule triggers an event that is pushed to your storage queue. 

Navigate to your storage browser and check **samplequeue** storage queue, and you should see the `MaintenancePreEvent` event.  

:::image type="content" source="media/maintenance-configurations-how-to-event-grid/samplepreevent.jpg" alt-text="Maintenance Pre Event in Storage Queue":::

## Event Grid event schema

Maintenance Configuration event data includes the `resourceUri`, which identifies the Maintenance Configuration resource that triggered the event. For details about the Maintenance Configuration event message schema, see the Event Grid documentation:

[Azure Event Grid event schema for API Management](../event-grid/event-schema-maintenance-configuration.md)

## Next steps

* [Choose between Azure messaging services - Event Grid, Event Hubs, and Service Bus](../event-grid/compare-messaging-services.md)
* Learn more about [subscribing to events](../event-grid/subscribe-through-portal.md).
