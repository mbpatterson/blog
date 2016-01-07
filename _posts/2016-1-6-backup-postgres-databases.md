---
layout: post
title: "Backup Postgres Databases"
date: 2016-1-6
categories: linux
---
SSH into your Postgres server and login as the postgres user `sudo su - postgres`.

Use the `pg_dumpall` command to make a backup of all datbases.

`pg_dumpall > filename.sql`

To backup a specific database use the following command

`pg_dump databaseName > filename.sql`

The backup will be saved in the `/var/lib/postgresql/` directory.
