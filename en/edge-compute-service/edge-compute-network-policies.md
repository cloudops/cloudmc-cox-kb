---
title: "StackPath:  Network Policies"
slug: stackpath-network-policies
---

StackPath **network policies** are the mechanism by which access to and from a workload is controlled.  Network policies can allow or block traffic to specific ports based on the source address, and are associated with their respective workload.

By default all incoming traffic to a workload is blocked, and all outgoing traffic from a workload is allowed.  When a workload is created, as a convenience the user may specify one or more ports to allow for incoming traffic, and StackPath will create the necessary network policies on the user's behalf.  Network policies may be added or deleted at any time during the life of a workload.

To create, modify, or delete a StackPath network policy, an account with the *User* role must be a member of the environment which contains the policy, and also have the *Editor* or *Owner* environment role assigned.  An account with the *Administrator* role or higher may create, modify, or delete policies in any environment.

To access network policies, navigate to your StackPath environment and click on the **Workloads** tab, click on the appropriate workload, and then click on the **Network policies** item.

### Default network policy

### Creating a network policy

1. From the **Network policies** screen, clicking click on the button labeled *Add network policy rule*.
1.

### See also

### External links
