---
title: "StackPath Service:  Overview of workloads"
slug: stackpath-overview-workloads
---


A StackPath **workload** represents the collection of resources needed to deliver an application.  A workload consists of one or more instances of either a virtual machine (a *VM*) or a container that can be deployed to one or more of StackPath's **edge locations** (points of presence, or **PoPs**).  A given workload can have one or more instances deployed at each PoP, all using the same image.  Virtual machines are built using an operating system image. Containers are built using a Docker image pulled from a registry such as DockerHub or other service.

At deployment, each instance is given a root volume whose size is defined by the image.  If additional storage is needed, a persistent volume may be attached to each virtual machine or container in a workload.  This is particularly useful for workloads using container instances, for which the root volume does **not** persist.

Every instance in a workload gets a private and public IP address upon creation.  [Network policies](stackpack-network-policies.md) control access to the instances in a workload.  For more complex deployments, an **Anycast IP address** can be added at the time of creation of the workload, and will remain assigned to the workload for the duration of its lifetime.  Anycast IP addresses are a feature of StackPath and provide optimal routing for traffic, essentially behaving as a global load balancer for your workload.  <!-- Need to show where Anycast IPs send traffic.  This is not clear to me at this time. -->

### External links

[Create and Manage Virtual Machines, Containers, and Workloads](https://support.stackpath.com/hc/en-us/articles/360022756051-Create-and-Manage-Virtual-Machines-Containers-and-Workloads)

[Edge Computing: Using Global Anycast IP Addresses](https://support.stackpath.com/hc/en-us/articles/360022801751-Edge-Computing-Using-Global-Anycast-IP-Addresses)

[Learn about Environment Variables](https://support.stackpath.com/hc/en-us/articles/360022768891)

[Edge Computing:  Enabling Auto-Scaling on Your Workload](https://support.stackpath.com/hc/en-us/articles/360034604611-Edge-Computing-Enabling-Auto-Scaling-on-Your-Workload)