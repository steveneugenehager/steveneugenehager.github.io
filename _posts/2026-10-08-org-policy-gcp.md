---
title: Implementing Org Policies in GCP via Terraform
tags: [gcp, cloud-setup, cloud-admin] 
---

Today I worked on implementing organization-level policies in GCP via Terraform. A new [repo](https://github.com/steveneugenehager/terraform-org-level-policy-gcp) was created for the need.

I extended the [bootstrap repo](https://github.com/steveneugenehager/terraform-bootstrap-project-gcp) to create a SA specifically for this need.
```
  account_id   = "tf-org-policy-admin"
  display_name = "Terraform organization policy automation"
```

For the first time I implemented a [GitHub action](https://github.com/steveneugenehager/terraform-org-level-policy-gcp/blob/main/.github/workflows/terraform-ci.yml) to run `terraform fmt` and `terraform validate` on commits.

To validate the organization policies are in place, use:
```
gcloud org-policies list --organization=<YOUR_ORG_ID>
```
I intended to paste example output here, but my "bastion" VM is stopped.

Other commands that might be useful for reviewing "organization policies" are:
```
# Show every available constraint, including the ones not set:
gcloud org-policies list --organization=<YOUR_ORG_ID> --show-unset
```
```
# Get the full detail of one policy as it’s stored:
gcloud org-policies describe iam.allowedPolicyMemberDomains \
  --organization=<YOUR_ORG_ID>
```
```
# Get the effective policy, which is what actually applies after inheritance and defaults
gcloud org-policies describe compute.vmExternalIpAccess \
  --organization=<YOUR_ORG_ID> --effective
```
```
# Check what a specific project sees, which is useful once folder overrides exist
gcloud org-policies list --project=<YOUR_PROJECT_ID>
gcloud org-policies describe compute.vmExternalIpAccess \
  --project=PROJECT_ID --effective
```
```
# Compare against what Terraform manages
terraform output managed_policies
```
