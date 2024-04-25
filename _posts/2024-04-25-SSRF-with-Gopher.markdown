---
layout: post
title:  PwnedLabs (GCP) - SSRF with Gopher
description: The second video in the GCP series in which the threat actor must leverage an SSRF vulnerability to exploit a misconfigured application. The application supports the gopher protocol which can be abused to query the metadata service. 
date:   2024-04-25
image: '/images/gopher_with_ssrf_web.png'
category: Videos
tags:   [GCP, SSRF, Storage Bucket, Videos]
---

This is the second video in the GCP series which showcases the Gopher SSRF scenario at https://pwnedlabs.io/labs/exploit-ssrf-with-gopher-for-gcp-initial-access. 

This scenario involves the exploitation of a web application using an SSRF vulnerability to obtain the GCP credentials from the web server's metadata. After obtaining the credentials, we access a storage bucket that the application interacts with to obtain sensitive data.

<iframe src="https://www.youtube.com/embed/UHRppAI2L20" frameborder="0" allowfullscreen></iframe>
[00:00](https://www.youtube.com/watch?v=UHRppAI2L20&t=0s) - Video context<br>
[01:00](https://www.youtube.com/watch?v=UHRppAI2L20&t=60s) - Discovering SSRF vulnerability<br>
[06:50](https://www.youtube.com/watch?v=UHRppAI2L20&t=410s) - Obtaining GCP credentials<br>
[18:17](https://www.youtube.com/watch?v=UHRppAI2L20&t=1097s) - Accessing the storage bucket<br>
[22:01](https://www.youtube.com/watch?v=UHRppAI2L20&t=1321s) - Post-compromise analysi<br>
