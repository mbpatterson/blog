---
layout: post
title: "Securing Websites with SSL"
date: 2016-1-9
categories: linux
---
Securing websites with SSL is almost a necessity today. Even for smaller websites it gives visitors peace of mind that their traffic is not being intercepted by a Man in the Middle attack and is absolutely necessary for sites that collect payments. It is fairly easy to generate a Certificate Signing Request (CSR), submit your CSR to validate your certificate, and set it on your web server or proxy.

The first step is to generate your CSR.

`openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr`

Alternatively you can specify a key length of `4096` which all modern browsers should support.

Fill out all the required information to complete the CSR.

When finished transfer the `xxxx.csr` file to your computer.

Upload this file to the CA you purchased a certificate from.

Make sure you have access to the `admin@yourdomain` email address for verification.

Once verified you will be emailed a .zip file containing 4 files (example uses COMODO certificates)

* AddTrustExternalCARoot.crt
* COMODORSADomainValidationSecureServerCa.crt
* COMODORSAAddTrustCA.crt
* your_cert_name.crt

Upload all of these back to your web/proxy server. and `cat` the first three into an `intermediate.bundle` file

`cat AddTrustExternalCARoot.crt COMODORSADomainValidationSecureServerCa.crt COMODORSAAddTrustCA.crt > intermediate.bundle`

Run `cat` again to combine the `intermediate.bundle` with `your_cert_name.crt` into `your_cert.pem` file

`cat intermediate.bundle your_cert_name.crt > your_cert.pem`

Copy `your_cert.pem` to `/etc/ssl/private/your.domain.name.pem` and make sure to set your web server to use this new certificate.
