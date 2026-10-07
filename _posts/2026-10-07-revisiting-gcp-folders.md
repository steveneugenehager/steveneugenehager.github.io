---
title: Org Folder Structure in GCP - Revisited
tags: [gcp, cloud-setup, cloud-admin] 
---

I decided to revisit my organization's folder structure in GCP. The [repo](https://github.com/steveneugenehager/terraform-folder-setup-gcp) was enhanced to add some top level folders (i.e., common, bootstrap), reduce the number of environments for now (e.g., lab, dev), introduce subfolders for the environment folders (to group infrastructure, services, and application domains), and demonstrate two application domain folders.

The following captures a snapshot of where the folders stand.  
![GCP folder hierarchy in the console]({{ '/assets/images/posts/2026/initial_folder_structure_gcp.jpg' | relative_url }})

Next, I am going to work on the "common" project (iam-ops) and SA to be used to provision users and add them to security groups. 