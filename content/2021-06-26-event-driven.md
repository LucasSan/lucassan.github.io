---
layout: post
title: The One with Event-Driven Architecture
date: 2021-06-26 21:56:20 +0300
description: Event-Driven architecture
img: workflow.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Javascript]
---

The Event-Driven Architecture grows in popularity in the last couple years, but how Lambda fits into the event-driven paradigm ?

The difference between event-driven and request-driven applications may not be clear in small applications but once your application develop more functionality and handle more traffic, this becomes more apparent.

Request-driven application typically use directed commands to coordinate downstream functions to complete an activity and are tightly, meanwhile Event-drive applications create events that are observable by other services and systems, but the event producer is unaware of which consumers, if any, are listening. Typically, there are loosely coupled.

Most Lambda-based applications use a combination of AWS services for durably storing data and integrating with other system and services. In these applications, Lambda acts as glue between the services, providing business logic to transform data as it moves between services

Building Lambda-based applications follows many of the best practices of building any event-based architecture. A number of development approaches have emerged to help developers create event-driven systems. Event storming, which is an interactive approach to domain-driven design (DDD), is one popular methodology. As you explore the events in your workload, you can group theses as bounded contexts to develop the boundaries of the micro services in your application.

Benefits of Event-Driven Architectures

- Replacing polling and web hooks with events
- Reducing complexity
- Improving scalability and extensibility

Trade-off:

- Variable latency
- Eventual consistency
- Returning values to callers
- Debugging across services and functions
