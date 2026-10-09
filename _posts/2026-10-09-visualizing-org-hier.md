---
title: Visualizing GCP organization/folder/project hierarchy
tags: [gcp, cloud-setup, cloud-admin] 
---

In today's work, I created a network host project in two environments in my infrastructure folders. (Will blog on that effort separately.) I wanted to capture a screen shot showing 
what my GCP organization's hierarchy was looking like, but even with four projects to date, the screen real estate on the GCP console is too small.

As a solution, I created [cookbook/gcp/org/gcp-tree.sh](https://github.com/steveneugenehager/cookbook/blob/main/gcp/org/gcp-tree.sh) that walks the hierarchy and displays it in a Linux window (with plain text):
```
Organization 822574087702
  + fldr-bootstrap/  (1091932959946)
      - shv-cld-admn-btstrp-4a71
  + fldr-common/  (696349522837)
      - shv-common-iam-ops-f1c1
  + fldr-dev/  (299189601259)
      + fldr-dev-domains/  (540841970714)
          + fldr-dev-customer/  (1096798476755)
          + fldr-dev-product/  (1089327451915)
      + fldr-dev-infrastructure/  (289051694142)
          - shv-dev-net-host-6ce3   [shared VPC host]
      + fldr-dev-services/  (727296981339)
  + fldr-lab/  (805819672746)
      + fldr-lab-domains/  (1068104658722)
          + fldr-lab-customer/  (757370703142)
          + fldr-lab-product/  (56486984661)
      + fldr-lab-infrastructure/  (57260783286)
          - shv-lab-net-host-6ce3   [shared VPC host]
      + fldr-lab-services/  (187800657244)
```
Obviously this script will be helpful as I add more domains and projects within existing domains.