---
layout: default
title: OAM Concepts
id: oam-concepts
---

{% include blocks/h1.html title="OAM concepts" %}

{% include blocks/h2.html title="Distributable vs. Distributed" %}

One of the key OAM concepts is application of distributability principle. OAM is not a distributed system in the context of high availability or general computation, but it does provide access and management for a wide range of distributed nodes in the OAM network. OAM is also not an implementation of another peer to peer network/computing but it is possible to move resources between OAM nodes.

Some OAM components are centralized to simplify management, but at the same time if we distribute those components they should be eventually consistent. For example, there is only one main Index service, and there are management commands that enable Index cloning and updating which does make any local Index eventually consistent.

One of the ways to implement a distributable system is to create a so called *append only log file* recording every change to the Index. The idea is to *replay* the append only log and file and recreate exact replica of the Index locally. An append only log file snapshot can be created at regular intervals and simply *fetched* by any OAM Node in the network.

{% include blocks/h2.html title="Authentication and authorization" %}

OAM is an open service, but it still requires some kind of authentication and authorization. For example, executing destructive management commands requires higher privileges than simply accessing imagery.

Since OAM is designed to be distributable, locally on a OAM Node a user has all the privileges and can do anything. This enables a functioning system in environments without the internet connection. However, any remote action, like Index or resource management will require checking the global OAM Index.

OAM will not store any passwords, users will be authenticated via external authentication providers over standard authentication protocols (OpenStreetMap OAuth1, Google OpenID, Github OAuth2, etc.). Once authenticated, internal authentication protocol, probably based on OAuth or something similar will be used. When a user registers and authenticates, OAM will generate a `user_token` and a `user_secret` identifiers, which will be used by internal protocol to authenticate and authorize actions on remote servers. `User_token` is part of the Index, however `user_secret` is only stored on the Master OAM Node.

For example, there are two OAM nodes. A user on Node1 wants to execute some kind of dataset processing on Node2, and the user has correctly specified his credentials on Node1. A management function on Node1 will use `user_token` and `user_secret` to authorize user on the Master OAM Node, which will return a `session_token`. Node1 will then execute a remote command on Node2 and use `user_token` and a `session_token` for authentication. Node2 will then use those tokens and check validity on Master OAM Node and at the same time invalidate `session_token`. The remote command will execute only if the `session_token` is valid and user has enough privileges, in the first place. Basically, every remote command execution needs to follow the same process.