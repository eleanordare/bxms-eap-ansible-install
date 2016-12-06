# bxms-eap-ansible-install

## Purpose
These playbooks install and configure BxMS 6.4 and EAP 7. Additional playbooks are included to apply future patches, configure roles in BPMS, and add custom login modules to Business Central.

## Getting Started
1. Make sure Ansible is installed on your host.
2. Update group_vars/all/all.yml to reflect your environment (and choose transfer method, see below).
3. Update inventory to reflect your environment.
4. Run `ansible-playbook bxms6.4-eap7.0-centos7-csp.yml` on host.

## Transfer Methods

These playbooks and their required roles support a few different mechanisms for transferring the product zip files to the target host. You can set the variable `transfer_method` to select the method, which can be set at multiple levels. The default is set in [group vars](https://github.com/eleanordare/bxms-eap-ansible-install/tree/master/group_vars/all/all.yml).

### csp-to-host
This method uses the [custom Red Hat_CSP_download module](https://github.com/sabre1041/redhat-csp-download) to download the product binaries from the [Red Hat Customer Portal](https://access.redhat.com/downloads/). This requires network access from the target host to the Red Hat Customer Portal, but has the advantage of being fully automated. Each of the roles have sensible defaults already set.

The following variables are required:
- `rhn_username`
- `rhn_password`

### copy-from-controller
This method requires you to download the product binaries from the [Red Hat Customer Portal](https://access.redhat.com/downloads/) and then add them to the files directory. This has the benefit of supporting installs when the target host does have public internet connectivity, but requires a manual step before the playbook can be run. Each of the roles have sensible defaults already set, and the .gitignore will ignore .zip files.

The easiest way to provide the files to your role is to symlink them to the role_name/files directory. If you files are not named the same as the files on the customer portal, you can override the file name using the `*_artifact_name` variables.


## Applying Patches
The apply_patch.yml tasks in roles/jboss_bxms and roles/jboss_eap can be updated with the appropriate patch download links from the [Red Hat Customer Portal](https://access.redhat.com/downloads/). To include the patch while running the playbook, change the `jboss_bxms_apply_patch` and `jboss_eap_apply_patch` variables to `true`. You can also run only the apply-patch tasks by adding the tag(s): `ansible-playbook bxms6.4-eap7.0-centos7-csp.yml --tags "jboss_bxms_patch,jboss_eap_patch"`.


## Red Hat Subscriptions

These playbooks require binaries from the [Red Hat Customer Portal](https://access.redhat.com/downloads/), so you'll need a valid subscription. Developers can get a $0 subscription through the [Red Hat Developer Program](http://developers.redhat.com/products/eap/download/).



## Credit
Adapted from the Red Hat Consulting [JBoss Middleware Playbooks](https://github.com/rhtconsulting/ansible-middleware-playbooks).
