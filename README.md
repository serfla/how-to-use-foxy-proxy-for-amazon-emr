# How to use foxy-proxy addon to view Web Interfaces hosted on Amazon EMR Cluster
This guide aims to support the users, who use the Amazon Elastic Map Reduce service, to access at the hadoop tools using foxy-proxy firefox addon.

## Why do you need this guide?
In the [management guide](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html) for Amazon EMR you can find all about that service. Now we focus on the section [Manage clusters](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-manage.html), where there are some helpful informations about web interface hosted on Amazon EMR cluster. In this section there are expose two options for accessing to the services hosted on the cluster master node:
- the first one explains how to use the ssh tunnel to access to a single service at a time 
- the second one explains how to set up a ssh tunnel to the cluster master node and access to  it via browser using the foxy-proxy firefox addon. 

It is in this last point where you find some trouble with foxy-proxy settings. In fact the part where is explained how to configure foxy-proxy is not clear. 

## Prerequisites
- an Amazon AWS account
- a ssh-client (for windows users the equivalent is [PuTTY](http://www.putty.org/)
- foxy-proxy firefox addon [(link)](https://addons.mozilla.org/firefox/addon/foxyproxy-standard/)

## First step
After create the amazon cluster, wait until it will be in a **WAITING** state.

Following the amazon guide, the first step is to create the tunneling to the master node of the cluster from your machine. For that step we can follow the amazon guide.

So, run the following shell command:

```bash
ssh -i cluster_pem_file.pem -ND 8157 hadoop@master-public-dns-name
```
> Note: 
> the **N** argument sets OpenSSH to not execute commands on the remote machine.
> The **D** argument sets a dynamic port forwarding which allows you to specify a local port used to forward data to all remote ports on the master node's local web server. Dynamic port forwarding creates a local SOCKS proxy listening on the port specified in the command. 

## Workarounds
