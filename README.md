
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
ssh -i myPem_file.pem -ND 8157 hadoop@master-public-dns-name
```


> Notes: 
- If the port number is already used in your system or is belong to the reserved range ports, the ssh command returns a error status.
- The **N** argument sets OpenSSH to not execute commands on the remote machine.
- The **D** argument sets a dynamic port forwarding which allows you to specify a local port used to forward data to all remote ports on the master node's local web server. Dynamic port forwarding creates a local SOCKS proxy listening on the port specified in the command. 

## Set up Foxy-proxy

Ok, now, we are going to set up the foxy-proxy addon, in order to we can access to amazon web interfaces.

After the cluster is ready to use, under the section **Connection** will appear a link **Enable web connection**, which will show you some informations about ssh tunneling and some foxyproxy settings as xml code.

![alt text](http://docs.aws.amazon.com/emr/latest/ManagementGuide/images/console-connect-tunnel-off.png "Enable web connection")

After to open the Enable Web Connection link, copy the xml code and paste it in a file named **foxyproxy-settings.xml** previously creating with a text editor.
You should see somethings like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<foxyproxy>
    <proxies>
        <proxy name="emr-socks-proxy" id="2322596116" notes="" fromSubscription="false" enabled="true" mode="manual" selectedTabIndex="2" lastresort="false" animatedIcons="true" includeInCycle="true" color="#0055E5" proxyDNS="true" noInternalIPs="false" autoconfMode="pac" clearCacheBeforeUse="false" disableCache="false" clearCookiesBeforeUse="false" rejectCookies="false">
            <matches>
                <match enabled="true" name="*ec2*.amazonaws.com*" pattern="*ec2*.amazonaws.com*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*ec2*.compute*" pattern="*ec2*.compute*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="10.*" pattern="http://10.*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*10*.amazonaws.com*" pattern="*10*.amazonaws.com*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*10*.compute*" pattern="*10*.compute*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*.compute.internal*" pattern="*.compute.internal*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*.ec2.internal*" pattern="*.ec2.internal*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
            </matches>
            <manualconf host="localhost" port="8157" socksversion="5" isSocks="true" username="" password="" domain="" />
        </proxy>
    </proxies>
</foxyproxy>
```

> Pay attention to set the **port** attribute value of **manualconf** tag with port number used for the ssh tunneling.

1. Choose Firefox > Add-ons.

2. On the **Add-ons** tab, to the right of FoxyProxy Standard, choose **Options**.

3. In the FoxyProxy Standard dialog, choose **New proxy**
4. In **Generals** tab, type the label (name) which identify the new proxy configuration. 
5. In the **Proxy details** tab, select **Manual proxy settings** and set-up the following fields:
    - **server name/IP address**: localhost
    - **port**: insert the port using for the ssh tunneling (**8157** in this example). 
    - check **SSL proxy?** and **Proxy SOCKS** boxes. 
    - verify if **SOCKS v5** is selected.
6. In the **URL or URL template** select **Import**
7. Browse to the location of **foxyproxy-settings.xml**, select the file, and choose Open.
8. Choose OK and restart Firefox.
9. When Firefox restarts, in the FoxyProxy Standard dialog, for Select Mode, choose Use proxies based on their pre-defined patterns and priorities.
10. Choose Close.

To open the web interfaces, in your browser's address bar, type **master-public-dns** followed by the port number or URL.

For a complete list of web interfaces on the master node, see [View Web Interfaces Hosted on Amazon EMR Clusters](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-web-interfaces.html).
## Workarounds
