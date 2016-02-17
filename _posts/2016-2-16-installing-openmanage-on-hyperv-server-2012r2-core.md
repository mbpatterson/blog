---
layout: post
title: "Installing OpenManage on HyperV Server 2012r2 Core"
date: 2016-2-16
categories: server
---
Installing the Dell OpenManage utility on a server core instance was not as difficult as it may appear. Extract the openamange executable file and inside there will be three folders. `docs`, `support`, and `windows`. Open and admin command prompt and change to the following directory. `....\openmanage\windows\SystemsManagementx64\` and run `msiexec -i SysMgmtx64.msi -qn` to run the silent OpenManage installer. It will take a few minutes and when finished you can point your browser to the server IP address on port 1311 to login to OpenManage.
