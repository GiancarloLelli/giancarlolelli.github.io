---
layout: post
title:  "Confidential computing : Introduzione"
date:   2022-11-07 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Spesso nei blog di quest'anno ho menzionato il termine Confidential Compunting e come sia un tema che sto approfondendo (e che sto provando a portare anche ad eventuali conferenze) ma mi sono reso conto che non ho mai dett o fatto una introduzione sul tema. Non sono una persona troppo prolissa, ma vi posso riassumere il Confidential Computing citando la definizione del Confidential Computing Consortium - un organo stile Cloud Native Computing Foundation che si occupa appunto del tema di cui stiamo parlando.

> Confidential computing is the protection of data in use using hardware-based Trusted Execution Environments (TEE). A Trusted Execution Environment is commonly defined as an environment that provides a level of assurance of data integrity, data confidentiality, and code integrity. A hardware-based TEE uses hardware-backed techniques to provide increased security guarantees for the execution of code and protection of data within that environment. Microsoft is one the key hyperscaler that are enriching its Cloud portoflio with solutions that allow us to run Confidential Computing workloads in Azure. A great session has been delivered at Ignite (you can watch on demand [here](https://ignite.microsoft.com/en-US/sessions/22182092-2662-41fe-a2a3-31131bcce6cc)) and you can have an overview of the current Microsoft Azure offering for Confidential Computing [here](https://techcommunity.microsoft.com/t5/azure-confidential-computing/bg-p/AzureConfidentialComputingBlog).

Una nozione tecnologica importante quando di parla di Confidential Computing è quella di Enclave o in generale della tecnologia Intel SGX

> Intel Software Guard Extensions (SGX) is a set of security-related instruction codes that are built into some Intel central processing units (CPUs). They allow user-level and operating system code to define protected private regions of memory, called enclaves. SGX is designed to be useful for implementing secure remote computation, secure web browsing, and digital rights management (DRM). Other applications include concealment of proprietary algorithms and of encryption keys. SGX involves encryption by the CPU of a portion of memory (the enclave). Data and code originating in the enclave are decrypted on the fly within the CPU, protecting them from being examined or read by other code, including code running at higher privilege levels such the operating system and any underlying hypervisors.