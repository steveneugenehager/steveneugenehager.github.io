---
title: Creating the first Org-level group in my GCP organization
tags: [gcp, cloud-setup, cloud-admin] 
---

I decided to create my first organization-level group in my GCP organization using Terraform.

In my [GCP bootstrap repo](https://github.com/steveneugenehager/terraform-bootstrap-project-gcp) I created a new Terraform file `groups.tf` to provision a group called `gcp-organization-admins@stevenhager.com`.

Org-level admin groups are few, small and tightly held. Google's setup checklist recommends a set, but I wanted to provision one of those first:

`gcp-organization-admins`: `Organization Admin`, `Folder Admin`, `Project Creator`

The Terraform plan was reviewed, and as expected the six adds were related to `group:gcp-organization-admins@stevenhager.com`:

```
Terraform will perform the following actions:

  # google_cloud_identity_group.admin["gcp-organization-admins"] will be created
  + resource "google_cloud_identity_group" "admin" {
      + additional_group_keys = (known after apply)
      + create_time           = (known after apply)
      + description           = "Org administrators: hierarchy, project creation, org policy"
      + display_name          = "gcp-organization-admins"
      + id                    = (known after apply)
      + initial_group_config  = "WITH_INITIAL_OWNER"
      + labels                = {
          + "cloudidentity.googleapis.com/groups.discussion_forum" = null
          + "cloudidentity.googleapis.com/groups.security"         = null
        }
      + name                  = (known after apply)
      + parent                = "customers/C017kium8"
      + update_time           = (known after apply)

      + group_key {
          + id = "gcp-organization-admins@stevenhager.com"
        }
    }

  # google_organization_iam_member.admin_groups["gcp-organization-admins|roles/billing.user"] will be created
  + resource "google_organization_iam_member" "admin_groups" {
      + etag   = (known after apply)
      + id     = (known after apply)
      + member = "group:gcp-organization-admins@stevenhager.com"
      + org_id = "822574087702"
      + role   = "roles/billing.user"
    }

  # google_organization_iam_member.admin_groups["gcp-organization-admins|roles/orgpolicy.policyAdmin"] will be created
  + resource "google_organization_iam_member" "admin_groups" {
      + etag   = (known after apply)
      + id     = (known after apply)
      + member = "group:gcp-organization-admins@stevenhager.com"
      + org_id = "822574087702"
      + role   = "roles/orgpolicy.policyAdmin"
    }

  # google_organization_iam_member.admin_groups["gcp-organization-admins|roles/resourcemanager.folderAdmin"] will be created
  + resource "google_organization_iam_member" "admin_groups" {
      + etag   = (known after apply)
      + id     = (known after apply)
      + member = "group:gcp-organization-admins@stevenhager.com"
      + org_id = "822574087702"
      + role   = "roles/resourcemanager.folderAdmin"
    }

  # google_organization_iam_member.admin_groups["gcp-organization-admins|roles/resourcemanager.organizationAdmin"] will be created
  + resource "google_organization_iam_member" "admin_groups" {
      + etag   = (known after apply)
      + id     = (known after apply)
      + member = "group:gcp-organization-admins@stevenhager.com"
      + org_id = "822574087702"
      + role   = "roles/resourcemanager.organizationAdmin"
    }

  # google_organization_iam_member.admin_groups["gcp-organization-admins|roles/resourcemanager.projectCreator"] will be created
  + resource "google_organization_iam_member" "admin_groups" {
      + etag   = (known after apply)
      + id     = (known after apply)
      + member = "group:gcp-organization-admins@stevenhager.com"
      + org_id = "822574087702"
      + role   = "roles/resourcemanager.projectCreator"
    }

Plan: 6 to add, 0 to change, 0 to destroy.
```

Upon completion, I could see the group using the command line:
```
sehager@HagerDell202511:~$ gcloud identity groups search \
  --organization=stevenhager.com \
  --billing-project="shv-cld-admn-btstrp-4329" \
  --labels="cloudidentity.googleapis.com/groups.discussion_forum"
groups:
- displayName: gcp-organization-admins
  groupKey:
    id: gcp-organization-admins@stevenhager.com
  name: groups/00vx12273cid2j3
  ```

  In the console, using <https://console.cloud.google.com/iam-admin/groups>, I could see the new group in my organization.

To validate the group is in fact a security group (as intended), I used:
```
sehager@HagerDell202511:~/$ gcloud identity groups describe gcp-organization-admins@stevenhager.com \
  --billing-project="shv-cld-admn-btstrp-4329"
additionalGroupKeys:
- id: gcp-organization-admins@stevenhager.com
- id: gcp-organization-admins@stevenhager.com.test-google-a.com
createTime: '2026-10-06T22:47:24.197882Z'
description: 'Org administrators: hierarchy, project creation, org policy'
displayName: gcp-organization-admins
groupKey:
  id: gcp-organization-admins@stevenhager.com
labels:
  cloudidentity.googleapis.com/groups.discussion_forum: ''
  cloudidentity.googleapis.com/groups.security: ''
name: groups/00vx12273cid2j3
parent: customers/C017kium8
updateTime: '2026-10-06T22:47:24.473703Z'
```