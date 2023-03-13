# Demo Onpremise Networking on Kubernetes

This repo contain one technique of networking orchestration in Kubernetes, bundled by using flux cd GitOps.

## Overview

We'll implement LoadBalancer native managed by k8s api, alright let's picked up MetalLB.  
short brief, MetalLB can announce `LB IP` by using `L2 ARP/NDP` or `L3 BGP`.

Skip it about BGP, it's more complex.  
If we announce LB IP by L2, we need to `NAT` Client or LB IP in order to communicate each others.

But `NAT` will make degraded performance, so we need to do routing or L3 cheating and outgoing traffic will handled by Cilium.  
Cilium will forward outgoing traffic direct to the client without pass to LB instance first in this case MetalLB.  
This technique called `DSR (Direct Server Routing)`.

So we have Chicken & Egg concept,  
You need to bootstrap CNI first cause Flux need it to do an reconciliation to this repo. Then Flux will handle your Cilium CNI resources for next update.

How to produce?
- Prepare the kubernetes distro, choose any derrivation what you need, prefer vanilla derrivative kubernetes
- Bootstrap CNI (Cilium)
- Bootstrap Flux

Detail versions,
- Kubernetes 1.24
- Cilium 1.12.7
- MetalLB 0.13.9
