---
title: What are the steps to validate a terraform script?
tags: [terraform]
---

From quickest to most thorough:

1. Format. Normalizes style and catches some syntax problems.
```
terraform fmt -check -diff        # report only, no changes (good in CI)
```
1. Initialize. Needed before first run.  Needed before validating whenever providers, modules or the backend change.
```
terraform init
```
1. Validate. Checks syntax, references, types and required arguments without calling any APIs. This catches undefined variables, misspelled resource references and wrong argument names. It won't catch problems that depend on real values or permissions.
```
terraform validate
```
1. Plan. The real test. It calls GCP and shows exactly what would change.  Read it carefully: the counts on the `Plan: X to add, Y to change, Z to destroy` line should match what you expect, and any `destroy` or `must be replaced` in a bootstrap config deserves a pause. Saving it with `-out` means `terraform apply tfplan` applies exactly what you reviewed.
```
terraform plan
```
1. `tflint` with the Google ruleset catches things validate misses, such as invalid machine types or deprecated arguments.
```
tflint --recursive           # include subdirectories/modules
```
1. `checkov` or `trivy config`. scan for security issues, such as overly broad IAM or public buckets.
```
checkov -d . --framework terraform   # Terraform checks only
```
1. `trivy` is an open-source security scanner from Aqua Security. It scans several kinds of targets: container images, filesystems, git repos, Kubernetes clusters and infrastructure-as-code. trivy config is the IaC mode.
```
trivy config --tf-vars terraform.tfvars .
```