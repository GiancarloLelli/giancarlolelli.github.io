---
layout: post
title:  "Reference implementation per architetture a Microservizi su Dynamics 365"
date:   2019-04-15 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Questo post sarà breve, ma servirà piuttosto come kickoff ufficiale di un mio nuovo pet project che ha l'obbiettivo di calare nel contetso Dynamics 365 il paradigma delle architetture a Microsoervizi. Grande buzz word ma sicuramente interessante da approfondire. Dynamics ha la fama di essere un bel mostro da domare e sicuramente avere un approccio strutturato ed industrializzato per creare applicazioni enterprise che lo utilizzano come backend ha un buon livello di valore. Ho creato un repository su [GitHub](https://github.com/GiancarloLelli/dynamics-365-microservice-reference-implementation) con qualche dettaglio sulla roadmap di questo PET project. I contributor sono bene accetti :)  

## README.MD
# Dynamics 365 on Microservices Reference Implementation
This sample ASP.NET Core application demonstrates how it is possible to apply the architecutral style known as *Microservice Architecture* for a Dynamics 365 custom application.
The application is build using the latest Microsoft technolgies and tries to provide an opinionated approach for the most common issues a developer eam faces when building a custom web app.
At the moment, all of this is a *work-in-progress* and there is no actual code pushed into this repo, however as the design evolves we will try to address the following concerns:

* Decomposition
* Authorization & Authentication
* API Composition
* Cross Service Communication
* External Services Integration
* Service Discovery
* Data Management
* Testing
* Logging & Monitoring
* Client-side UI Composition
* CI/CD
* Containerization
* Long running task

### About & License
This repository is distributed under the [GPLv3](https://choosealicense.com/licenses/gpl-3.0/) license I ([Giancarlo Lelli](https://twitter.com/itsonlyGianca)) am the only maintainer.
For information about the project such as "how to get involved" reach out to me. Many of the concepts and reasong behind this project are taken from this awesome tutorial
about microservice available at this [address](https://microservices.io/patterns/microservices.html)
