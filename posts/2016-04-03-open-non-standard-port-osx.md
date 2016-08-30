---
title: Open access to "non-standard" port in OSX with pf
tags: osx, elasticsearch
---

In an increasingly interconnected world of containers and IoT, the need to closely monitor and normalize requests in and out of a system
has become ever more important. For example, suppose you collect statistics from some sensor in your home automation system and you want to send it back to
an Elasticsearch container running inside your Macbook for later analysis. To accomplish this task, you need to configure OSX to
allow incoming requests from the sensor on port 9200 (the default Elasticsearch port) and route it to the container. Fortunately, from OSX 10.7 onwards,
this task is trivial with the help of a powerful tool called `pf`, which stands for Packet Filter, originated from FreeBSD.

[To be updated]
