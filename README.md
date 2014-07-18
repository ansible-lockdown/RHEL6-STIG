RHEL 6 DISA STIG
================

Configure RHEL 6 machine to be DISA STIG compliant. CAT I findings will be corrected by default. CAT II and CAT III findings can be corrected by setting the appropriate variable.

This role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

When the role is running, you will most likely see a good number of failures. This is caused by using the `shell` module to run `grep` and look for output. If not output is returned, the `shell` module will fail. However, this is usally a good sign because no output means no finding.

The role tries to be helpful and pause to let you know it found something. You can disable this behaviour if you want to run it unattended by setting `fullauto` to `true`.

Based on [Red Hat Enterprise Linux 6 Security Technical Implementation Guide 2013-06-03](http://www.stigviewer.com/stig/red_hat_enterprise_linux_6/).

Inspiration and some config files taken from [RedHatGov](https://github.com/RedHatGov) [stig-fix-el6](https://github.com/RedHatGov/stig-fix-el6).

Requirements
------------

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.

Role Variables
--------------

**rhel6_stig_cat1**:           Correct CAT I findings (Default: true)

**rhel6_stig_cat2**:           Correct CAT II findings (Default: false)

**rhel6_stig_cat3**:           Correct CAT III findings (Default: false)

**rhel6_stig_snmp_community**  borealis

**fullauto**                   Run the role without pausing (Default: false)

**root_email_address**          Address where system email is sent (Default: foo@baz.com)

**xwindows_required**           Whether or not X Windows is is use on taregt systems. Disables some changes if X Windows is not in use.

**pass_min_length**             Minimum password length (Default: 14)

**pass_min_days**               Minimum password age in days (Default: 1)

**pass_max_days**               Maximum password age in days (Default: 60)

Dependencies
------------

Ansible > 1.6

Example Playbook
-------------------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: disa-stig-rhel6, rhel6_stig_cat1: true, rhel6_stig_cat2: true, rhel6_stig_cat3: false }

License
-------

MIT

