---
layout: post
title: "Setup HyperV Server 2012 Without Domain"
date: 2016-2-15
categories: misc
---
I a in the process of redoing my home lab and settled on HyperV as my virtualization platform. I have experience using it for work and want to increase my knowledge and skills in configuring and using it. Setting up HyperV Server 2012 without a domain was a bit more difficult than I anticipated. Read on to see all the steps needed to get this done...

I created a 50GB virtual disk to house the OS. Since this is a GUI-less install that amount of space should be more than sufficient. After installing and logging in you will see a blue window with 14 menu options to choose from. This is called Server Configuration and can be accessed if ever closed by typing `sconfig` from the command line.

First change the hostname of the system and then configure remote management to be enabled and Remote Desktop to be enabled. Next update the local Windows Firewall rules to allow remote management on the necessary ports. Since this is a lab environment I just disabled Windows Firewall completely. `Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`

Next make sure WinRM is enabled on both your management computer and the server. `Get-Service winrm` If it is not running you need to start it using `Start-Service winrm`. Also enable PowerShell remoting by running `Enable-PSRemoting -force`. Since the management computer and the server are not on a domain, you need to add the computer to the list of trusted hosts in WinRM. `winrm s winrm/config/client '@{TrustedHosts="RemoteServer"}'` This should be done on both the management computer and the server.

Last run `dcomcnfg` as administrator on the management computer to open the Windows Component Services manager. Expand **Component Services** -> **Computers** and right click on **My Computer**. Click on **Properties** and select the **COM Security** tab. Under the **Access Permissions** section click on the **Edit Limits** button. Select the **Anonymous Logon** group and check **Remote Access**.

Now you should be able to connect HyperV manager to your server as well as PowerShell and Server Manager!
