# Odoo application stack for Kubernetes on Wodby

Deploy Odoo Community Edition on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for Odoo.

<!-- wodby:generated:start -->

## Stack contract

- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [Odoo service](https://github.com/wodby/service-odoo)
- [Nginx (Odoo) service](https://github.com/wodby/service-odoo-nginx)
- [PostgreSQL service](https://github.com/wodby/service-postgres)
- [NFS provisioner service](https://github.com/wodby/service-nfs-provisioner)
- [OpenSMTPD service](https://github.com/wodby/service-opensmtpd)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Odoo<br>`odoo` | required; enabled by default; volumes: `filestore` 20 GB; links: `db` → `postgres`, `filestore` → `filestore-nfs`, `sendmail` → `opensmtpd` |
| Nginx<br>`nginx` | required; enabled by default; main; links: `backend` → `odoo` |
| PostgreSQL<br>`postgres` | required; enabled by default; volumes: `data` 20 GB |
| Odoo data NFS storage<br>`filestore-nfs` | required; enabled by default; volumes: `data` 25 GB |
| OpenSMTPD<br>`opensmtpd` | optional; disabled by default |

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifest

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

<!-- wodby:generated:end -->

## Deploy this stack

The stack needs no repository: Odoo and every Community app come with the prebuilt
[`wodby/odoo`](https://github.com/wodby/odoo) image. Install apps from Odoo's Apps menu.

Enter the administrator's password, and optionally a login, email, language and the apps to install, in the Odoo
service's settings when you create the app. The first deployment installs Odoo with that account; Odoo's database
manager is turned off. Later deployments update the modules that a new image changed.

Uploaded files and sessions are kept on the shared `filestore` volume, so Odoo can run more than one replica.
Enable OpenSMTPD, or set up an outgoing mail server in Odoo, to send email.

## Maintain a custom version

1. Fork this repository.
2. Edit the stack manifest.
3. Import the repository as a [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target
and derivative reference. Stack-local names and referenced service names are
distinct identifiers.
