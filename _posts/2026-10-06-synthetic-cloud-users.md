---
title: Munging Synthetic Users for Cloud Demonstration
tags: [python, simulation]
---

I created an additional [repo](https://github.com/steveneugenehager/cloud-tools) for one time tasks related to synthetic data generation and validation of such.

I am planning on creating a series of users in my GCP organization and in my local Linux VM and place some users in various org-level roles now and other users in domain/project level roles later (as those projects are deployed).

The file `cloud-users/synthetic-cloud-users.txt` (<https://github.com/steveneugenehager/cloud-tools/blob/main/cloud-users/synthetic-cloud-users.txt>) contains names that will be used to create `first_last@mydomain.com` users in GCP and `first_last` Linux accounts using `useradd` on my local VM. With the accounts I will be able to access and interact with the GCP console as a typical user would. I.e., one in a job function and given specific roles. With the accounts I will be able to execute command line activities using a account mapped to a GCP account with limited scope.

`cloud-users/validate_users.py` (<https://github.com/steveneugenehager/cloud-tools/blob/main/cloud-users/validate_users.py>) runs a sanity check on each of the planned users. You supply the data file on the command line like:
```
~/cloud-tools/cloud-users$ ./validate_users.py synthetic-cloud-users.txt
```
