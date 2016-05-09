---
layout: post
title: "JIRA and Confluence Setup Part 3: Proxy Setup"
date: 2016-5-8
categories: atlassian
---
In part 3 we will go over how to setup the proxy server that will handle SSL termination and forward our requests to JIRA and Confluence. I am using HAProxy but nginx will also work. Total setup time should be under 45 minutes.

HAProxy is not included in the default Ubuntu Server software repositories so we need to add an additional one. Switch to the root user then add the repository with the commands below:

    sudo su
    add-apt-repository ppa:vbernat/haproxy-1.5

Next run `apt-get update` and `apt-get install haproxy` to install.

Before we edit the HAProxy configuration file, we need to generate a CSR for the SSL certificates JIRA and Confluence will need. Be sure to do this twice, once for JIRA and once for Confluence.

    openssl req -new -newkey rsa:4096 -nodes -keyout yourdomain.key -out yourdomain.csr

Make sure to fill out every required field when prompted.

Purchase 2 SSL certificates (the most basic ones are fine) from your provider of choice (I use Namecheap) and upload the generated CSR. Enter the appropriate administrator email for domain validation. When finished, you should receive 2 files in an email for each certificate. One will be your certificate in .CRT format along with a ca-bundle. SFTP these to your proxy server.

Once uploaded, you need to `cat` the correct files for each domain into a single .pem file for HAProxy to read.

    cat jira.yourdomain.key >> jira.yourdomain.pem
    cat jira.yourdomain.crt >> jira.yourdomain.pem
    cat ca-bundle >> jira.yourdomain.pem

Copy the `yourdomain.pem` file to `/etc/ssl/private/yourdomain.pem`

Repeat these steps for the Confluence certificate.

Next we need to edit the HAProxy configuration file.

    sudo vi /etc/haproxy/haproxy.cfg

Under **global configuration** set `maxconn` to 2048

Under **defaults** add `option forwardfor` and `option http-server-close`

Frontend configurations need to be created for both http and https connections:

    frontend http
    bind your.ip.address:80
    reqadd X-Forwarded-Proto:\http

    acl host_jira hdr(host) -i jira.yourdomain.com
    acl host_confluence hdr(host) -i confluence.yourdomain.com

    use_backend jira-backend if host_jira
    use_backend confluence-backend if host_confluence

    frontend https
    bind your.ip.address:443 crt /etc/ssl/private/jira.yourdomain.pem crt /etc/ssl/private/confluence.yourdomain.pem
    reqadd X-Forwarded-Proto:\https

    acl host_jira hdr(host) -i jira.yourdomain.com
    acl host_confluence hdr(host) -i confluence.yourdomain.com

    use_backend jira-backend if host_jira
    use_backend confluence-backend if host_confluence

Backend configurations need to be created for both JIRA and Confluence telling HAProxy where to forward incoming connections.

    backend jira-backend
    redirect scheme https if !{ ssl_fc }
    server jira jira.internal.ip:8080 check

    backend confluence-backend
    redirect scheme https if !{ ssl_fc }
    server confluence confluence.internal.ip:8090 check

Finally, restart the HAProxy service to load your new configuration

    sudo service haproxy restart

[Part 1: Server Planning](http://michaelpatterson.me/atlassian/2016/05/01/jira-confluence-setup-part-1.html)

[Part 2: Database Setup](http://michaelpatterson.me/atlassian/2016/05/02/jira-confluence-setup-part-2.html)

[Part 3: Proxy Configuration](http://michaelpatterson.me/atlassian/2016/05/08/jira-confluence-setup-part-3.html)

Part 4: JIRA Installation

Part 5: Confluence Installation
