# Foreword

Discussion

# Overview

This project is related to Anthropic's Model Context Protocol (MCP, see [docs](https://modelcontextprotocol.io) and [spec](https://spec.modelcontextprotocol.io/)).

MCProxy is a proxy between MCP Clients and MCP Servers, introducing new features in the workflow between them (a few examples are given below).

# Global architecture

From a Client's perspective, MCProxy behaves like a Server.
From a Server's perspective, MCProxy behaves like a Client.

In its simplest form:

```mermaid
flowchart LR
    client1[MCP Client]
    subgraph "MCProxy"
        server1[MCP Server]
        client2[MCP Client]
    end
    server2[MCP Server]

    client1 <--MCP--> server1
    client2 <--MCP--> server2
    server1 <--> client2
```

A MCProxy can connect to multiple servers:

```mermaid
flowchart LR
    client1[MCP Client]
    subgraph "MCProxy"
        server1[MCP Server]
        client2[MCP Client A]
        client3[MCP Client B]
    end
    server2[MCP Server A]
    server3[MCP Server B]

    client1 <--MCP--> server1
    client2 <--MCP--> server2
    client3 <--MCP--> server3
    server1 <--> client2
    server1 <--> client3
```

A MCP Host may connect to different MCProxies with very different configurations:

```mermaid
flowchart LR
    subgraph "Host"
        clientH1[MCP Client]
        clientH2[MCP Client]
    end
    subgraph "MCProxy A"
        serverPA[MCP Server]
        clientPA[MCP Client]
    end
    serverA[MCP Server A]
    subgraph "MCProxy B"
        serverPB[MCP Server]
        clientPB[MCP Client]
    end
    serverB[MCP Server B]

    clientH1 <--MCP--> serverPA
    clientH2 <--MCP--> serverPB
    clientPA <--MCP--> serverA
    clientPB <--MCP--> serverB
    serverPA <--> clientPA
    serverPB <--> clientPB
```


If of any use, MCProxies can be chained:

```mermaid
flowchart LR
    client1[MCP Client]
    subgraph "MCProxy A"
        server1[MCP Server]
        client2[MCP Client]
    end
    subgraph "MCProxy B"
        server2[MCP Server]
        client3[MCP Client]
    end
    server3[MCP Server]

    client1 <--MCP--> server1
    client2 <--MCP--> server2
    client3 <--MCP--> server3
    server1 <--> client2
    server2 <--> client3
```

# Internal architecture


# Features

Here are a few examples of features an MCProxy could implement.

## Logging

An MCProxy can log all messages going back and forth between an MCP Client and an MCP Server.

## Capabilities aggregation

An MCProxy can aggregate capabilities from different servers into a meaningful package.

## Capabilities blocking

An MCProxy may expose only a subset of the capabilities provided by the server(s) it connects to.

## Capabilities disambiguation

With the fast-growing number of servers, me might end up with some name collisions. An MCProxy may rename capabilities, so that they appear unique to a client, and handle routing to the appropriate server.
