---
layout: post
title:  PwnedLabs (GCP) - Hidden Files
description: The first video in the GCP series features a scenario where participants are provided with a URL leading to a misconfigured storage bucket serving image files, prompting them to fuzz potential files, discover a backup zip file due to the entity being set to "Public" with "allUsers" granted Reader access, and completing the challenge by decrypting the zip file.
date:   2024-04-08
image: '/images/hidden_files_web.png'
category: Videos
tags:   [GCP, Storage Bucket, Videos]
---

This is the first video in the GCP series. This video showscases the first GCP scenario at https://pwnedlabs.io/. 

The only information that is provided to complete this challenge is the URL of a web application which is serving image files from a misconfigured storage bucket. After fuzzing the potential files in the storage bucket, a backup zip file was discovered because the object's entity is Public with "allUsers" being given the Reader access. After decrypting the zip file the challenge is complete.

<iframe src="https://www.youtube.com/embed/wGklP-4jAYI" frameborder="0" allowfullscreen></iframe>
[00:00](https://www.youtube.com/watch?v=wGklP-4jAYI&t=0s) - Video context 
[00:39](https://www.youtube.com/watch?v=wGklP-4jAYI&t=39s) - Discovering storage bucket 
[03:29](https://www.youtube.com/watch?v=wGklP-4jAYI&t=209s) - Finding hidden file 
[05:07](https://www.youtube.com/watch?v=wGklP-4jAYI&t=307s) - Decrypting file 
[08:05](https://www.youtube.com/watch?v=wGklP-4jAYI&t=485s) - Post-compromise analysis


