# Foreword

Discussion

# Overview

This project is related to Anthropic's Model Context Protocol (MCP, see [docs](https://modelcontextprotocol.io) and [spec](https://spec.modelcontextprotocol.io/)).

MCProxy is a proxy between MCP Clients and MCP Servers, introducing new features in the workflow between them (a few examples are given below).

# Global architecture

From a Client's perspective, MCProxy behaves like a server.
From a Server's perspective, MCProxy behaves like a client.

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
