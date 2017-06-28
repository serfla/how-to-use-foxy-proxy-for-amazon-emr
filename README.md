# how to use foxy-proxy addon to view Web Interfaces hosted on Amazon EMR Cluster
This guide aims to support the users, who use the amazon Elastic Map Reduce service, to access at the hadoop tools using foxy-proxy firefox addon.

## Why do you need this guide?
In the [management guide](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html) for Amazon EMR you can find all about that service. Now we focus on the section [Manage clusters](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-manage.html), where there are some helpful informations about web interface hosted on Amazon EMR cluster. In this section there are expose two option for accessing to  the services hosted on the cluster master node:
- the first one explains how to use the ssh tunnel to access to a single service at a time 
- the second one explains how to set up a ssh tunnel to the cluster master node and access to  it via browser using the foxy-proxy firefox addon. 

## Prerequisites

- an amazon AWS account
- a ssh-client (for windows user the equivalent is the puttty)
- a cluster EMR created 

Note
## First step
create an EMR cluster

