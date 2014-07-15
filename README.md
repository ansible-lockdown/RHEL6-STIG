Role Name
========

Configure RHEL 6 machine to be DISA STIG compliant. CAT I findings will be corrected by default. CAT II and CAT III findings can be corrected by setting the appropriate variable.

Based on [Red Hat Enterprise Linux 6 Security Technical Implementation Guide 2013-06-03](http://www.stigviewer.com/stig/red_hat_enterprise_linux_6/). 

Requirements
------------

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.

Role Variables
--------------

**rhel6_stig_cat1**:           Correct CAT I findings (Default: true)

**rhel6_stig_cat2**:           Correct CAT II findings (Default: false)

**rhel6_stig_cat3**:           Correct CAT III1 findings (Default: false)

**rhel6_stig_snmp_community**  borealis

Dependencies
------------

None.

Example Playbook
-------------------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: disa-stig-rhel6, rhel6_stig_cat1: true, rhel6_stig_cat2: true, rhel6_stig_cat3: false }

License
-------

MIT

