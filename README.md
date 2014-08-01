RHEL 6 DISA STIG
================

Configure RHEL 6 machine to be DISA STIG compliant. CAT I findings will be corrected by default. CAT II and CAT III findings can be corrected by setting the appropriate variable to enable those playbooks.

This role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

When the role is running, you will most likely see a good number of failures. This is caused by using the `shell` module to run `grep` and look for output. If not output is returned, the `shell` module will fail. However, this is usally a good sign because no output means no finding.

The role tries to be helpful and pause to let you know it found something. You can disable this behaviour if you want to run it unattended by setting `rhel6stig_fullauto` to `true`.

Based on [Red Hat Enterprise Linux 6 Security Technical Implementation Guide 2013-06-03](http://www.stigviewer.com/stig/red_hat_enterprise_linux_6/).

Inspiration and some config files taken from [RedHatGov](https://github.com/RedHatGov) [stig-fix-el6](https://github.com/RedHatGov/stig-fix-el6).

Requirements
------------

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.

Role Variables
--------------

**rhel6stig_cat1**:           Correct CAT I findings (Default: true)

**rhel6stig_cat2**:           Correct CAT II findings (Default: false)

**rhel6stig_cat3**:           Correct CAT III findings (Default: false)

**rhel6stig_snmp_community**  borealis

**rhel6stig_fullauto**                   Run the role without pausing (Default: false)

**rhel6stig_root_email_address**          Address where system email is sent (Default: foo@baz.com)

**rhel6stig_xwindows_required**           Whether or not X Windows is is use on taregt systems. Disables some changes if X Windows is not in use.

**rhel6stig_pass_min_length**             Minimum password length (Default: 14)

**rhel6stig_pass_min_days**               Minimum password age in days (Default: 1)

**rhel6stig_pass_max_days**               Maximum password age in days (Default: 60)

**rhel6stig_ipv6_in_use**       Whether or not ipv6 is in use of the target system. This is set automatically to 'true' if ipv6 is found to be in use. (Default: false)

**rhel6stig_tftp_required**  Whether or not TFTP is required. This will prevent the removal of `tftp` and `tftp-server` packages. It will also  reconfigure the `tftp-server` to run securely. (Default: false)
Dependencies
------------

Ansible > 1.6

Example Playbook
-------------------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: disa-stig-rhel6, rhel6stig_cat1: true, rhel6stig_cat2: true, rhel6stig_cat3: false }

License
-------

MIT

