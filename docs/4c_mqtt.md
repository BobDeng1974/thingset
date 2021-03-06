---
title: "MQTT"
---

# ThingSet to MQTT mapping

The MQTT protocol doesn't support the request/response part of the ThingSet protocol, but the publish/subscribe messaging pattern can be easily mapped.

## Publication

The topic used to publish to the MQTT server may contain the path of the data nodes.

It is possible to publish a single measurement value to a single topic or publish multiple child nodes of a path to the same MQTT topic. The selected approach depends on the application. For the ThingSet protocol less configuration nodes are needed if multiple data nodes are gathered under one MQTT topic.

It is recommended that only the JSON or CBOR payload data is stored in the MQTT payload and the path of the ThingSet endpoint is contained in the topic.

## Subscription

Content fetched from a specific topic is forwarded to the ThingSet protocol as a publication message (containing the topic or parts of it). If the ThingSet device has the subscription to the specified topic enabled, it will parse the incoming data similar to a PATCH request. Unknown data nodes contained in the subscribed payload are silently ignored.

If the application requires some processing of incoming data before applying them to actual nodes, temporary data nodes can be created where the subscribed content is written to. Afterwards a function is called (assigned as a callback to the sub channel node) to process the incoming data and pass it on to the actual nodes.

This approach can also be used to determine if the received data has changed compared to already existing state and avoid too many storage operations in EEPROM for regularly published messages.
