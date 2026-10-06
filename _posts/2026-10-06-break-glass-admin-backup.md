---
title: Creating a Backup BreakGlass Super Admin in your new GCP Organization
tags: [gcp, cloud-setup, cloud-admin] 
---

In my [GCP bootstrap repo](https://github.com/steveneugenehager/terraform-bootstrap-project-gcp), I have created
`create_secondary_org_admin.sh` (in the `follow-on` directory; [Location in GitHub](https://github.com/steveneugenehager/terraform-bootstrap-project-gcp/blob/main/follow-on/create_secondary_org_admin.sh)) to address the need for a backup break-glass super admin user in my new GCP organization.


## It does three things, in order:
1. Creates the user in Workspace/Cloud Identity through the Directory API. (If the user already exists, it skips this step.)
1. Grants Super Admin with `users/{email}/makeAdmin`. In GCP terms, Super Admin plus Org Admin is what makes an account an "organization owner." The call retries a few times because a newly created account can take a moment to propagate.
1. Binds org-level IAM roles with gcloud organizations `add-iam-policy-binding`. The default is `roles/resourcemanager.organizationAdmin`, and you can pass others with `--org-roles`.

## Some things to know before running it:

`gcloud` can't manage Workspace users. That's why steps 1 and 2 call the Admin SDK REST API with curl and an ADC token. You run the script as an existing Super Admin (the initial one used to create the organization), so it doesn't need a service account or domain-wide delegation. 

What is an "ADC token"? ADC stands for Application Default Credentials. It's the standard way Google client libraries, Terraform, and other tools find credentials automatically, so you don't hard-code a key in your code. An "ADC token" is the OAuth access token minted from whichever credential ADC finds.

The OAuth consent screen may block the login. Google sometimes rejects the default `gcloud` client when it asks for Admin SDK scopes ("This app is blocked"). If that happens, create a Desktop OAuth client in the bootstrap project and add `--client-id-file=client_secret.json` to the application-default login line. I encountered this and in my implementation inclused the passing of the JSON generated using the console. 

Run `--dry-run` first. It prints every API call without the password and makes no changes.

If you choose not to "prompt for the password", handle the temporary password carefully. The script writes it to a mode-600 temp file and never prints it. Sign in right away, change the password, enroll a hardware security key, and shred the file. For a true break-glass account, also keep it out of SSO and day-to-day use, and set up alerting on its sign-ins.  In my example run, I specified it during the script's run and all worked well. The steps involving a random password and those steps weren't taken (or tested).

## Why is a break-glass backup super admin account in GCP for a organization recommended?
In a Google Cloud org, the Super Admin in Cloud Identity/Workspace is the root of everything, and if you only have one, you have a single point of failure with no one above you to fix it. GCP has no "call support and prove you own the account" path that works quickly or reliably for a small org.

The main reasons:
1. Lockout recovery. If your only Super Admin loses its second factor (a phone dies, a security key is lost), forgets the password, or gets suspended by Google's automated abuse detection, nobody else can reset it. A second Super Admin can reset the first in minutes. Google's own recovery process for a sole Super Admin involves domain-ownership verification (adding DNS records) and can take days.
1. Recovering from your own mistakes. It's easy to lock yourself out with a policy change: enforcing SSO through an IdP that then breaks, enabling 2SV enforcement before enrolling, tightening context-aware access, or removing your own Org Admin IAM binding. A break-glass account that's deliberately excluded from those policies is how you get back in.
1. Compromise response. If your daily admin account is phished or its session hijacked, an attacker can lock you out by changing the password and 2SV. A separate account with its own hardware key, rarely used and closely monitored, lets you revoke the attacker's sessions and take the org back.
1. IdP or SSO outages. If you federate sign-in to Okta, Entra ID, or similar, an outage there blocks all your admins. Super Admins can bypass SSO, and the break-glass account should be set up to sign in directly with Google credentials.
1. Separation of daily work from root power. Best practice is not to do everyday work as Super Admin at all. Your daily account gets scoped admin roles and IAM roles; the Super Admin credentials stay in the safe. A dedicated break-glass account makes that split practical.
1. Bus factor. For a team, it ensures the org survives one person leaving or being unavailable. For a personal org like yours, it mostly protects you from yourself and from lost devices.

What makes it "break-glass" rather than just a second admin is how it's handled: long random password and hardware security keys (ideally two, stored separately), no email forwarding or day-to-day use, excluded from SSO and restrictive access policies, sign-in alerts so any use is noticed, and a periodic test login (say, quarterly) to confirm it still works. Google recommends at least two, but not many more, Super Admins for exactly this balance: enough for recovery, few enough to keep the attack surface small.


I executed the script and was able to successfully provision the backup SUPER ADMIN using the command line.

```
sehager@HagerDell202511:~/terraform-bootstrap-project-gcp$ cd follow-on
sehager@HagerDell202511:~/terraform-bootstrap-project-gcp/follow-on$ ./create_secondary_org_admin.sh \
       --domain "stevenhager.com" \
       --username breakglass-admin \
       --given "Break" --family "Glass" \
       --project "shv-cld-admn-btstrp-4329" \
       --org-roles "roles/resourcemanager.organizationAdmin,roles/billing.admin" \
       --client-secret "client_secret_<uniqueToYourOrg>.apps.googleusercontent.com.json" \
       --prompt-password
==> Enabling Admin SDK API on shv-cld-admn-btstrp-4329
==> Checking whether breakglass-admin@stevenhager.com exists
Password for breakglass-admin@stevenhager.com (min 16 chars):
Confirm password:
==> Creating breakglass-admin@stevenhager.com
Sleeping here for 30 seconds...
==> Granting Super Admin to breakglass-admin@stevenhager.com
==> Resolving GCP organization for stevenhager.com
==> Organization: 822574087702
==> Binding roles/resourcemanager.organizationAdmin on organization 822574087702
Updated IAM policy for organization [822574087702].
==> Binding roles/billing.admin on organization 822574087702
Updated IAM policy for organization [822574087702].

==> Done: breakglass-admin@stevenhager.com is Super Admin and holds: roles/resourcemanager.organizationAdmin,roles/billing.admin
!! Sign in now as breakglass-admin@stevenhager.com and enroll a hardware security key.
```

Using <https://admin.google.com/ac/users> I could see the user has been provisioned.

I was able to login to <https://console.cloud.google.com/> as the user and navigate to my organization, folders, and projects.

### Further nots
I did add a "sleep 30" to avoid a condition where a subsequent step would fail due to propagation delays.

I did have to wrangle a bit to get the authorization to work via the script.  I'll make a another post on my learnings about wslview.