---
layout: post
title: "JIRA and Confluence Setup Part 1: Server Planning"
date: 2016-5-1
categories: atlassian
---
JIRA and Confluence are powerful tools to help manage projects and write documentation. The 10 user starter license is only $10 per year per product and is a fantastic value. This is the first post in a multi-post series detailing how to get these products installed and configured.

[Part 1: Server Planning](http://michaelpatterson.me/atlassian/2016/05/01/jira-confluence-setup-part-1.html)

[Part 2: Database Setup](http://michaelpatterson.me/atlassian/2016/05/02/jira-confluence-setup-part-2.html)

[Part 3: Proxy Configuration](http://michaelpatterson.me/atlassian/2016/05/08/jira-confluence-setup-part-3.html)

Part 4: JIRA Installation

Part 5: Confluence Installation


Technically, JIRA and Confluence can be configured to run together on only one server, but separating all of the duties onto different servers makes for more secure and performant systems. We are going to be creating 4 linux servers with each one handling a specific function.


**PROXY** - Will run HAProxy and forward traffic to the correct application as well as handle SSL termination.

**DB** - Will host the PostgreSQL database JIRA and Confluence write to.

**JIRA** - Application server for JIRA

**CONFLUENCE** - Application server for Confluence


For this tutorial these will all be running Ubuntu 14.04 as the operating system, but most flavors of Linux should also work. As far as server resources are concerned, both the proxy and database servers can get by with a single CPU and a minimal amount of ram (512 megabytes is plenty). The two application servers require a bit more ram (1+ gigabytes are recommended), and although they can run on a single CPU, performance will be better with 2+. I am hosting these as virtual machines on an old computer in my house but I have also used virtual private servers (VPS) from Digital Ocean. Any VPS provider that offers both public and private networking will work.

I won't go into any further details of server setup as it varies greatly depending on how you choose to host them. I will likely have future posts which better describe my specific setup and will link to them from here.
