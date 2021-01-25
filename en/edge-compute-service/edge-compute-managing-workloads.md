---
title: "StackPath Service:  Managing workloads"
slug: stackpath-managing-workloads
---


<!-- General note:  UI changes have been made for hiding the optional attributes as well as for displaying the Anycast IP.  I need to go back and update the articles for these changes, both languages. -->

To create, modify, or delete a StackPath workload, an account with the *User* role must be a member of the environment which contains the workload, and also have the *Editor* or *Owner* environment role assigned.  An account with the *Administrator* role or higher may create, modify, or delete workloads in any environment.

To access workloads, navigate to your StackPath environment and click on the **Workloads** tab.

### Add a workload

#### Basic attributes

1. From the **Workloads** tab, click on the *Add workload* button.
1. Enter a name for the workload.
1. Select the type of workload:
   - Virtual machine
   - Container

#### Select image

1. If the type of workload is **Virtual machine** (**VM**), select an operating system image from the popup list labeled **OS image**.
1. If the type of workload is **Container**, enter into the text box labled **OS image** the URL from which the container image should be pulled.  The format of the URL should be `URL/imagename:tag`, where *tag* indicates the version to pull. If *tag* is omitted, `default` is assumed.  If credentials are required for the remote location, check the box labeled *Add image pull credentials*.  Fields for the credentials will appear.

#### Environment variables and network

1. If the type of workload is **Container**, environment variables may be defined and made accessible to the container.  These values are defined at runtime or during deployment, and are presented within the container as standard environment variables. <!-- The SP// docs seem to indicate that environment variables are available to both containers and to instances.  Also, how can multiple variables be defined in the Web UI? -->
   - **Environment variable key and value**:  Key value pairs are made available to all instances.  
   - **Secret environment variable key and value**: Same as environment variables but can be used to store credentials securely.
1. Select the VPC to use for deploying the workload.
   - At this time, workloads are created only in the default VPC.
1. If an Anycast IP address is desired, mark the checkbox labeled *Add Anycast IP address*.
1. If the public ports that the application will need are known, they can be specified by checking the box labeled *Add public port*, and filling in the fields that appear.  StackPath will create the network policies on your behalf at the time of deployment.  These can be edited at any time after the workload is deployed.

#### Initial startup configuration

1. If the type of workload is **Virtual machine**, at least one public key must be specified in the field labeled *First boot SSH key(s)*.  The key (or keys) will be automatically installed for the default user account in virtual machines created for this workload.
   - When connecting to the VMs via SSH, specify the private key that corresponds to the public key pasted here.  To find default usernames used by StackPath, see [Add Users to a Virtual Machine](https://support.stackpath.com/hc/en-us/articles/360025308732-Add-Users-to-a-Virtual-Machine).  Password-based login is not supported.
1. If the type of workload is **Container**, the text field labeled **Commands** may be used to specify commands to execute when the container starts. <!-- Docs say that multiple commands can be given, how are they separated, semi-colon? Comma-separated?  The API docs seem to indicate an array. -->

#### Compute and storage options

1. Select the compute and memory offering to be allocated for the workload from the popup menu labeled *Spec*.  The offering will be uniform for all VMs or containers deployed in all PoPs for this workload.
1. An additional storage volume may be attached to each instance or container:
   - Enter a path (such as `/data`) in the field labeled *Persistent storage path*.  At the time of deployment, StackPath will create a disk and the specified path will be used as the mountpoint inside the VM or container.
   - Select the desired size for the persistent volume using the slider labeled *Persistent storage size*.

#### Deployment name and target locations

1. Enter a name to describe the deployment in the field labeled *Deployment name*.
1. The target edge location (or locations) for deploying the workload is specified in the field labeled *Deployment PoPs*.  Click on the menu and list of the available PoPs will appear.  Clicking on a PoP will add a tag to the field.  To remove a selected PoP, click on the "X" on the right of the tag, and the workload will not be deployed there.

#### Workload scaling

1. Select the number of VMs or containers to deploy in each PoP using the slider labeled *Instances per PoPs*.
1. Alternatively, auto-scaling may be used in lieu of a static number of VMs or containers.  Check the box labeled *Enable auto scaling* and select the desired values for the criteria to trigger auto-scaling using the three sliders that appear:
   - *CPU utilization*: StackPath will monitor the CPU utilization of each instance or container.  When utilization exceeds the specified threshold, a new VM or container will be deployed to absorb the increased load.  When CPU utilization drops to 10% below this threshold or lower, StackPath will scale down instances after 5 minutes have passed since the last auto-scaling event.
   - *Min instances per PoP*: The minimum number of VMs or containers to have deployed at any time.
   - *Max instances per PoP*: The maximum number of VMs or containers to deploy when scaling up.

Once all options have been selected, click *Submit*. The **Workloads** tab will appear, and the new workload will be listed in the **Active** state.

### List existing workloads

All workloads are listed in the **Workloads** tab.  Clicking on an entry will display the *Workload Details* page, which lists the attributes for the selected workload.

Clicking on the **Instances** item will present the *Instances* page, where all VMs or containers for the workload are listed.  See [Stackpath: Working with instances](stackpath-working-with-instances.md).

Clicking on the **Network policies** item will present the *Network policies* page, where all inbound and outbound rules for this workload appear.  See [Stackpath: Network policies](stackpath-network-policies.md).

### Edit a workload

Edit a workload from the **Workloads** tab by finding the desired workload and clicking on the three-dot *Action* menu on the right side of the entry and clicking *Edit*, or from the *Workload details* page by clicking on the three-dot *Action* menu at the top right of the page.

Some attributes cannot be modified after deployment. The name of the workload and deployment may be changed, as well as the compute offering specification.  The deployment PoPs, instances per PoP, and auto-scaling rules may modified.

Once the modifications are complete, click *Submit* to save.  When adding a deployment PoP or changing the number of instances, it may take a few minutes for the new edge location to be listed and for VMs and containers to appear.

### Delete a workload

Deleting a workload will destroy all instances, containers, network policies, and persistent storage.  This action **cannot** be undone.

1. From the **Workloads** tab, find the desired workload and click on the three-dot *Action* menu on the right side of the entry.  A workload may also be deleted from its *Workload details* page by clicking on the three-dot *Action* menu at the top right of the page, and clicking *Delete*.
1. A dialogue box will appear, asking for confirmation to delete the workload.  Click *Submit*.
1. The workload will change to the **Disabled** state.  When the workload has been deleted, it will disappear from the **Workloads** tab.

### External links

[Add Users to a Virtual Machine](https://support.stackpath.com/hc/en-us/articles/360025308732-Add-Users-to-a-Virtual-Machine)
