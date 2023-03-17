---
layout: post
title:  "Azure Dev annoucements @ Ignite 2021"
date:   2021-11-08 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Settimana scorsa si è consluso Ignite 2021 e devo dire che ci sono stati una serie di annunci lato Azure Dev & Ecosystem che mi sono davvero piaciuti tanto. In basso trovate un riepilogo:

* Azure Communication Services SMS short code preview and Microsoft Teams interoperability GA
* Azure Container Apps is now in preview
* Azure Logic Apps: including running logic apps disconnected, managed identity support and more
* Azure Service Bus large message support GA
* Azure Web PubSub service reaches general availability
* New solutions for running Java EE applications on Azure container platforms
* Open Service Mesh add-on for Azure Kubernetes Service now GA
* Azure DevOps: OpenID Connect integration and a DevOps Workflow Generator tool
* Updates to Azure API management include GraphQL preview, WebSocket API general availability and more

Ti questa lista quello che credo esplorerò di più in questi prossimi giorni è quello relativo alle **Container Apps** che possiamo riassumere come segue:

> A serverless application centric hosting service where users do not see or manage any underlying VMs, orchestrators, or other cloud infrastructure. Azure Container Apps enables executing application code packaged in any container and is unopinionated about runtime or programming model. Applications can scale in response to HTTP requests, events (e.g. storage queue messages, Kafka topics, etc.), or simply run as always-on background jobs. Azure Container Apps addresses specific requirements for microservices including encrypted service to service communication and the independent versioning and scaling of services. Azure Container Apps is built on the foundation of powerful open-source technology in the Kubernetes ecosystem . The open-source centric approach enables a path for teams to build microservices without having to overcome the concept and operational overhead of working with Kubernetes directly and enables continued application portability by leveraging open standards and APIs. Behind the scenes, every application runs on Azure Kubernetes Service, with Kubernetes Event Driven Autoscaling (KEDA), Distributed Application Runtime (Dapr), and Envoy deeply integrated in the hosting service.