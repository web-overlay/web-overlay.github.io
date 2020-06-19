---
layout: single
title: Simple Unstructured Overlay Network
classes: wide
---

[Web-Overlay](/) includes an implementation of simple [unstructured overlay
network](https://en.wikipedia.org/wiki/Peer-to-peer#Unstructured_networks)
as an sample program.

This is a very simple unstructured overlay network.

- it runs on a Node.js and only has a CUI interface.
- it tries to keep at least 3 connections.
- it supports random walking and flooding.

# How to play
## Join
Open several terminal windows.

On the 1st terminal, run the initial node listening on port 8000.

```
% cd packages/unstructured
% node dist/unode.js http://localhost:8000
Started: NodeID=99d7428d, URL=http://localhost:8000
Command Line Usage:
status              -- show connections
put key value       -- store a key-value pair in this node
rw [--ttl=N] key    -- search key by single random walk
flood [--ttl=N] key -- search key by flooding
99d7428d> 
```

On the 2nd terminal, run the 2nd node that listens on port 8001.
http://localhost:8000 is used as the URL of the introducer node.
The 2nd node joins the overlay network of the 1st node.

```
% cd packages/unstructured
% node dist/unode.js http://localhost:8001 http://localhost:8000
```

Join more nodes likewise.

## CUI

This program provides a simple CUI.

- "status" command shows connections.
- "put" command puts key-value pair in the local node.
- "flood" command searches by flooding.
- "rw" command searches by random walking.

Example:

Let us assume that there is 2 nodes, N1 and N2, in the overlay
network.

Put some value in each node.

```
N1> put Japan Tokyo
```

```
N2> put Japan Osaka
```

Search by flooding.  TTL (max hops) value can be changed with
```--ttl``` option.

```
N1> flood Japan
TTL=3
Found value Tokyo at N1 (path=Path(N1))
Found value Osaka at N2 (path=Path(N1->N2))
Query Timeout
N1> flood --ttl=0 japan
TTL=0
Found value Tokyo at N1 (path=Path(N1))
Query Timeout
N1>
```

Search by random walking.

```
N1> rw Japan
TTL=3
Found value Tokyo at N1 (path=Path(N1))
N1>
```

Note that random walking finds at most single entry.

# Complete Source Code

The complete source code is included in the `packages/unstructured`
directory.

You can view it online at [https://github.com/abelab/web-overlay/blob/master/packages/unstructured/src/unode.ts](https://github.com/abelab/web-overlay/blob/master/packages/unstructured/src/unode.ts).

<div style="text-align: right;">
Last updated: Fri Jun 19 14:50:12 JST 2020
</div>

<!-- Local Variables: -->
<!-- coding: utf-8 -->
