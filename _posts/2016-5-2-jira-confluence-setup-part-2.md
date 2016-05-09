---
layout: post
title: "JIRA and Confluence Setup Part 2: Database Setup"
date: 2016-5-2
categories: atlassian
---
In part 2 we will go over how to setup the database JIRA and Confluence will both use. I am using PostgreSQL but MySQL and SQL Server will also work. Total setup time should be under 30 minutes.

First you need to install PostgreSQL. Since I am using Ubuntu Server, the following two commands need to be run in order to install.

    sudo apt-get update
    sudo apt-get install postgresql

Next, login as the newly created postgres user and create the JIRA and Confluence database roles

    sudo -su postgres

    createuser jira --interactive
    Superuser = No
    Create Databases = Yes
    Create additional roles = No

Repeat these commands for the confluence role

Next you need to set the role passwords and create the databases

    psql

    ALTER ROLE jira WITH PASSWORD 'password';
    ALTER ROLE confluence WITH PASSWORD 'password';
    CREATE DATABASE jiradb OWNER jira;
    CREATE DATABASE confluencedb OWNER confluence;

Entering `\list` should show both of the new databases.

Last you need to edit two configuration files to tell postgres which network interface to listen on and to allow connections from your two application servers.

First, edit the `postgresql.conf` file

    sudo vi /etc/postgresql/9.3/main/postgresql.conf

Uncomment the line `listen_address` and set it to * to tell postgres to listen on all network interfaces.

    listen_address = '*'

Second, edit the `pg_hba.conf` file

    sudo vi /etc/postgresql/9.3/main/pg_hba.conf

Add the address/subnet for the hosts that will be connecting to the database

    # IPv4 local connections
    host    all    all    your.jira.server.ip/32    md5
    host    all    all    your.confluence.server.ip/32    md5

Finally, restart the postgres service.

    sudo service postgresql restart

[Part 1: Server Planning](http://michaelpatterson.me/atlassian/2016/05/01/jira-confluence-setup-part-1.html)

[Part 2: Database Setup](http://michaelpatterson.me/atlassian/2016/05/02/jira-confluence-setup-part-2.html)

[Part 3: Proxy Configuration](http://michaelpatterson.me/atlassian/2016/05/08/jira-confluence-setup-part-3.html)

Part 4: JIRA Installation

Part 5: Confluence Installation
