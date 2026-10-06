---
title: Creating an Organization in GCP
tags: [gcp, cloud-setup, cloud-admin] 
---

I decided to pursue setting up an organization in GCP for the following reasons:
1. It is required to create shared VPCs,
1. to create folders to hold projects based on a hierarchy
    1. environment (e.g, lab, development, nonproduction, production)
    1. IT area - infrastructure, data systems, application domains 
1. demonstrate  organization-level policy implementation
1. demonstrate  folder-level policy implementation

## Register a domain
I bought stevenhager.com through [Squarespace](https://www.squarespace.com/) on a 1-year plan with auto-renewal turned off. Its status can be checked at 
<https://account.squarespace.com/domains>. At this time I am not planning to host a website there. I expect my experimentation can be executed on GCP with articles posted on GitHub.

## Sign up for Cloud Identity
Using Google's sign-up page for GCP identity, <https://workspace.google.com/signup/gcpidentity/welcome>, it was simple and straightforward to verify that I owned stevenhager.com and created the initial GCP admin account, admin@stevenhager.com.

Once the domain was verified, Google created the organization "Steve Hager Ventures" for it.  You can see the organization ID using the IAM page in the console, <https://console.cloud.google.com/iam-admin/>.

You can also use the command line:
``` bash
gcloud organizations list --format="value(ID)"
```

Using this GCP admin account, I then built the bootstrap GCP project using Terraform.  That effort will be described in a subsequent post.
