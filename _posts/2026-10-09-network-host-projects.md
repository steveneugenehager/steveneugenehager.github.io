---
title: Implementing Network Host Projects in GCP via Terraform
tags: [gcp, cloud-setup, cloud-network] 
---

As part of my demo of a GCP build, I have intended to create a VPC per environment in a "network only" (aka network host) project for that environment.

Today, as I starting working on that step, I decided I wanted to implement a "no Compute" policy on these types of projects, now.  At first Claude suggested implementing the deny policy on the infrastructure folder, but that wasn't my intention. I'd certainly expect some infrastructure-related projects would be leveraging COMPUTE, so this needs to be accomplished at the project level.  To accomplish this, a project tag looked to be the best approach.

Some changes were necessary in prior stages (aka repos) to enable this tag management.
1. [bootstrap](https://github.com/steveneugenehager/terraform-bootstrap-project-gcp)
1. [org policy](https://github.com/steveneugenehager/terraform-org-level-policy-gcp)

Changes were made to [network host projects](https://github.com/steveneugenehager/terraform-network-project-setup-gcp) and these projects were implemented via Terraform.

I validated the two projects are known to GCP as host-projects in my organization:
```
~$ gcloud compute shared-vpc organizations list-host-projects ORG_ID --project=PROJECT_ID
NAME                   CREATION_TIMESTAMP  XPN_PROJECT_STATUS
shv-lab-net-host-6ce3
shv-dev-net-host-6ce3
```

Tomorrow I will return to the stage to create the VPCs. The projects are empty for now.
```
~$ gcloud compute networks list --project=shv-dev-net-host-6ce3
Listed 0 items.
~$ gcloud compute networks list --project=shv-lab-net-host-6ce3
Listed 0 items.
```

I may rename the terraform-vpc-setup-gcp repo (to terraform-network-vpc-setup-gcp ) to match the naming of terraform-network-project-setup-gcp.

There is a static capture of the org/folder/project hierarchy [here]({% post_url 2026-10-09-visualizing-org-hier %}).  It shows these two newly created projects in each environment's infrastructure folder.