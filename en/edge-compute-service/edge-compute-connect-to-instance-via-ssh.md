---
title: "Connect to a Cox Edge Compute instance via SSH"
slug: edge-compute-connect-to-instance-via-ssh
---


An instance in a Cox Edge Compute workload of type VM may be reached using a standard SSH client.

### Prerequisites

1. You must have the private key counterpart of the public key that was used when the VM was created.  The private key was generated with the public key, and is usually a file that begins with the string `-----BEGIN RSA PRIVATE KEY-----`.
1. There must be an inbound network rule in *Network policies* for this workload, allowing TCP traffic originating from the IP that you are connecting from, on port 22.
1. You must know the default username for the operating system running on the VM.  See *List of default usernames* below.

### Connect to the instance

#### Identify the IP address

1. Navigate to the environment containing the desired workload.
1. Click on the workload to see its details.
1. Click on the *Instances* item on the left.  A list of the instances deployed in the workload will appear.
1. Identify the desired instance, and note its public IP address in the list.  Clicking directly on the IP address will copy it to your clipboard.
1. You are now ready to connect to the instance.  The procedure is different when using a standard Unix command line client versus a Windows SSH client.

#### Standard Unix command line SSH client

The private key should be present in your `.ssh` directory.  The file containing the private key must have permissions of `0644`.  To set the correct permissions, use the command `chmod 644 <filename>`.

The general form for using a private key to connect to a remote host via SSH is: `ssh -i <keyfile> username@IP`. The key must be specified with a path if the key is not in the current working directory.

For example, if connecting to 151.139.189.1 and the key file is in `.ssh/my.key`, use the following command:

```
ssh -i my.key username@151.139.189.1
```

You should now be connected to the VM and see a command prompt.  If root privileges are required, use the command `sudo -i` to get a root shell.

#### Windows SSH client

This a generic procedure for using a Windows SSH client.  See the user manual for your SSH client for the exact procedure.

**Note:** When connecting to your instance with a Windows SSH client, it may be necessary to first convert the private key and then install the key into the client.  See the user manual for your SSH client for details on configuring SSH keys.

1. Open a new connection in your SSH client.
1. Select **SSH** as the connection type.
1. Enter the IP address of the VM into the **Host Name** (or **IP address**) field.
1. Enter the default username into the **Username** or **Login as** field.
1. At this point, you will have to supply the private key or have it already installed, as mentioned in the note above.  See the user manual for your SSH client.
1. Click **Connect** or **Open**.
1. You should now be connected to the VM and see a window with a command prompt.  If root privileges are required, use the command `sudo -i` to get a root shell.

### List of default usernames

| OS name | Default username |
| --- | --- |
| CentOS | centos |
| Debian | debian |
| Ubuntu | ubuntu
