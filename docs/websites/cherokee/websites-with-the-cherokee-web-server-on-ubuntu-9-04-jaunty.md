---
deprecated: true
author:
  name: Linode
  email: docs@linode.com
description: 'Instructions for getting started with the Cherokee web server on Ubuntu 9.04 (Jaunty).'
keywords: 'cherokee,web sever,cherokee on ubuntu,ubuntu jaunty'
license: '[CC BY-ND 3.0](http://creativecommons.org/licenses/by-nd/3.0/us/)'
alias: ['web-servers/cherokee/installing-cherokee-ubuntu-9-04-jaunty/']
modified: Friday, April 29th, 2011
modified_by:
  name: System
published: 'Monday, September 14th, 2009'
title: 'Websites with the Cherokee Web Server on Ubuntu 9.04 (Jaunty)'
---



Cherokee is a fast, flexible web server for POSIX compliant operating systems such as Linux. It's designed to be easy to administer, and includes support for a wide range of common web server functions.

This tutorial explains how to install and configure the Cherokee web server on Ubuntu 9.04 (Jaunty). This guide targets Cherokee version 0.11.6 (provided by the Ubuntu Jaunty repositories as of this writing), but the install steps should be similar for later versions. We will be performing the installation through the terminal; please make sure you are logged into your Linode as root via SSH.

This document assumes that you already have a working and up to date Ubuntu 9.04 (Jaunty) system. If you have not followed our [getting started](/docs/getting-started/) guide, we recommend that you do so prior to following these instructions.

Check Package Sources
---------------------

First, make sure you have the `universe` repositories enabled on your system. Your `/etc/apt/sources.list` should resemble the following (you may have to uncomment or add the `universe` lines):

{: .file-excerpt }
/etc/sources.list
:   ~~~
    ## main & restricted repositories
    ~~~

    > deb <http://us.archive.ubuntu.com/ubuntu/> jaunty main restricted deb-src <http://us.archive.ubuntu.com/ubuntu/> jaunty main restricted
    >
    > deb <http://security.ubuntu.com/ubuntu> jaunty-security main restricted deb-src <http://security.ubuntu.com/ubuntu> jaunty-security main restricted
    >
    > \#\# universe repositories deb <http://us.archive.ubuntu.com/ubuntu/> jaunty universe deb-src <http://us.archive.ubuntu.com/ubuntu/> jaunty universe deb <http://us.archive.ubuntu.com/ubuntu/> jaunty-updates universe deb-src <http://us.archive.ubuntu.com/ubuntu/> jaunty-updates universe
    >
    > deb <http://security.ubuntu.com/ubuntu> jaunty-security universe deb-src <http://security.ubuntu.com/ubuntu> jaunty-security universe

If you had to enable new repositories, issue the following command to update your package lists:

    apt-get update
    apt-get upgrade

Next, we'll get Cherokee installed and configured.

Install Cherokee
----------------

Enter the following command to install the Cherokee web server, its documentation and a collection of utilities.

    apt-get install cherokee 

Several packages will be installed in addition to the main server package. You may visit your Linode's IP address (or domain name, if you have it pointed to the IP) in a web browser to verify that Cherokee is running. You should see the default Cherokee test page.

Configuring Cherokee
--------------------

The Cherokee web server includes an easy to use, comprehensive administration interface. This interface, known as `cherokee-admin`, is the recommended means of administering your web server.

Start `cherokee-admin` by issuing the following command:

    cherokee-admin -b & 

This instructs the administration program to bind to all IP addresses so it may be reached remotely. It will be launched in the background, so you'll still be able to use your SSH session. You should see output similar to the following:

    root@archimedes:~# cherokee-admin -b &
    [1] 3319
    root@archimedes:~# 
    Login:
      User:              admin
      One-time Password: YJ0cfZMSGw9B34vl

    Cherokee Web Server 0.11.6 (Jan  2 2009): Listening on port 9090, TLS disabled,
    IPv6 disabled, using epoll, 1024 fds system limit, max. 505 connections,
    single thread

### Secure Admin Panel Access

Instead of binding to all interfaces on your Linode, you may wish to bind to localhost and use SSH port forwarding to securely reach the administration system from your workstation. To do so, issue the following commands to launch `cherokee-admin` and set up an SSH tunnel. The first command is not required if you haven't already launched `cherokee-admin`. You may need to install the `killall` command first by issuing `apt-get install psmisc` on your VPS.

On your Linode:

    killall cherokee-admin 
    cherokee-admin & 

In a terminal window on your local workstation (MacOS X, Linux, BSD, etc) :

    ssh -L 9090:localhost:9090 root@12.34.56.78 -N 

Replace "12.34.56.78" with your Linode's IP address. You may now [visit cherokee-admin](http://localhost:9090) in your web browser browser via the SSH tunnel. To stop the tunnel, simply press `Ctrl+C` in your local terminal window.

You'll be presented with the Cherokee administration panel, which you may use to configure websites and specify configuration options. You'll still need to log in using the username and one-time password provided when you launched `cherokee-admin`.

[![The cherokee-admin web server administration interface running on a Linode.](/docs/assets/458-cherokee-ubuntu-jaunty-admin-01-home.png)](/docs/assets/458-cherokee-ubuntu-jaunty-admin-01-home.png)

### Secure Admin Panel Access on Windows

You can use [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) to set up a secure SSH tunnel for Cherokee administration. Enter your Linode's public IP address in the session tab:

[![Session information for PuTTY.](/docs/assets/459-cherokee-putty-01-session.png)](/docs/assets/459-cherokee-putty-01-session.png)

Expand the "Connection -\> SSH" menus to select the "Tunnel" configuration page. Enter the values shown here:

[![Configuring an SSH tunnel for Cherokee administration in PuTTY.](/docs/assets/460-cherokee-putty-02-tunnel.png)](/docs/assets/460-cherokee-putty-02-tunnel.png)

Back on the session tab, enter "Cherokee Admin" in the "Saved Sessions" field and click "Save" to retain your settings for future use.

[![Saving an SSH session for Cherokee administration in PuTTY.](/docs/assets/461-cherokee-putty-03-saved-session.png)](/docs/assets/461-cherokee-putty-03-saved-session.png)

Click "Open" to connect to your server and start the tunnel. You may receive a warning similar to the one shown here:

[![An SSH key alert in PuTTY.](/docs/assets/462-cherokee-putty-04-alert.png)](/docs/assets/462-cherokee-putty-04-alert.png)

Click "Yes" to continue, and log into your Linode as you normally would. As long as the SSH session is open you'll be able to navigate to `http://localhost:9090` in your web browser to access the Cherokee admin panel via the secure tunnel.

Conclusion
----------

Be sure to stop `cherokee-admin` using the `killall` command shown above once you're done configuring your system. Congratulations, you've successfully installed the Cherokee web server on your Linux VPS!

More Information
----------------

You may wish to consult the following resources for additional information on this topic. While these are provided in the hope that they will be useful, please note that we cannot vouch for the accuracy or timeliness of externally hosted materials.

- [Cherokee Web Server Documentation](http://www.cherokee-project.com/doc/)



