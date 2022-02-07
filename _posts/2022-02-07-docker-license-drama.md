---
layout: post
title:  "Docker license update"
date:   2022-02-07 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Oggi ahimè mi sono scontrato con i licensing changes di Docker. Nello specifico:
> * We’re introducing a new product subscription, Docker Business, for organizations using Docker at scale for application development and require features like secure software supply chain management, single sign-on (SSO), container registry access controls, and more.
> * Our Docker Subscription Service Agreement includes a change to the terms for Docker Desktop:
Docker Desktop remains free for small businesses (fewer than 250 employees AND less than $10 million in annual revenue), personal use, education, and non-commercial open source projects.
> * It requires a paid subscription (Pro, Team or Business), starting at $5 per user per month, for professional use in larger businesses. You may directly purchase here, or share this post and our solution brief with your manager.
> * While the effective date of these terms is August 31, 2021, there is a grace period until January 31, 2022 for those that require a paid subscription to use Docker Desktop.
> * Docker Pro, Docker Team, and Docker Business subscriptions include commercial use of Docker Desktop.
> * The existing Docker Free subscription has been renamed Docker Personal.
> *No changes to Docker Engine or any upstream open source Docker or Moby project.

Il team IT dell'azienda ha già provveduto a mandare una comunicazione interna per raccomandarsi di associare ad una istallazione di Docker anche un WBS, pena la disinstallazione automatica del tool...sigh :/