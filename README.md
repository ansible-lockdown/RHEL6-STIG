RHEL 6 DISA STIG
================
[![Galaxy](https://img.shields.io/badge/galaxy-nousdefions.STIG--RHEL6-blue.svg?style=flat)](https://galaxy.ansible.com/nousdefions/STIG-RHEL6)

Configure RHEL 6 to be DISA STIG compliant. CAT I findings will be corrected by default. CAT II and CAT III findings can be corrected by setting the appropriate variable to enable those tasks.

Not all findings can be remediated automatically, or they require more complex automation specific to your environment in order to be remediated appropriately. See `tasks/not_automated.yml` for these findings.

This role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted, though auditing functionality is in the works.

## IMPORTANT INSTALL STEP ##

To install this role with `ansible-galaxy` use the following command:

`ansible-galaxy install -p roles nousdefions.STIG-RHEL6,devel`

Based on [Red Hat Enterprise Linux 6 STIG Version 1 Release 6 - 2015-01-23](http://iase.disa.mil/stigs/os/unix-linux/Pages/index.aspx).

Inspiration and some config files taken from [Frank Caviggia](https://github.com/fcaviggia/hardening-script-el6).

This repo originated from work done by [Sam Doran](https://github.com/samdoran/ansible-role-rhel6stig)

Requirements
------------

You should have a general understanding of the nature of the changes this role will make to the system. See the [DISA  IASE site](http://iase.disa.mil/stigs/os/unix-linux/Pages/index.aspx) for details<a href="#fn1" id="note1">[1]</a>.

Role Variables
--------------
There are many role variables defined in defaults/main.yml. Here are the most important ones. Feel free to look through `defaults/main.yml` to see what other configuration options are available.

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `rhel6stig_cat1` | True | Correct CAT I findings |
| `rhel6stig_cat2` | False | Correct CAT II findings |
| `rhel6stig_cat3` | False | Correct CAT III findings |
| `rhel6stig_fullauto` | True | Run the role without pausing |
| `rhel6stig_use_dhcp` | True | Whether the system should use DHCP or Static IPs. **Setting this False is dangerous**. |
| `rhel6stig_system_is_router` | False | Whether on not the target system is acting as a router. Disables settings that would break the system if it is a acting as a router |
| `rhel6stig_root_email_address` | `foo@baz.com` | Address where system email is sent. |
| `rhel6stig_xwindows_required` | False | Whether or not X Windows is is use on taregt systems. Disables some changes if X Windows is not in use. |
| `rhel6stig_ipv6_in_use` | False | Whether or not ipv6 is in use of the target system. This is set automatically to 'true' if ipv6 is found to be in use. (Default: false) |
| `rhel6stig_tftp_required` | False |  Whether or not TFTP is required. This will prevent the removal of `tftp` and `tftp-server` packages. It will also  reconfigure the `tftp-server` to run securely. |
| `rhel6stig_change_grub_password` | False | Whether or not to update the grub password even if a hash already exists in `/boot/grub/grub.conf`. |
| `rhel6stig_bootloader_password` | [Randomly generated and encrypted string] | The new grub password to use if `rhel6stig_change_grub_password` is **True** |


Dependencies
------------

None.

Example Playbook
-------------------------

Correct CAT I and CAT II findings.

```yaml
- hosts: all
  sudo: yes

  roles:
    - { role: RHEL6-STIG,
        rhel6stig_cat1: True,
        rhel6stig_cat2: True,
        rhel6stig_cat3: False
      }
```

Prompt for the GRUB password.

```yaml
- hosts: servers
  sudo: yes

  vars:
    rhel6stig_cat1: True
    rhel6stig_cat2: True
    rhel6stig_cat3: False

  vars_prompt:
    name: "rhel6stig_bootloader_password"
    prompt: "Enter the bootloader password: "
    private: yes
    confirm: yes

  roles:
     - role: RHEL6-STIG
```


Tags
----
Each task is tagged with its category, severity, whether or not it is a patch or audit task, and the finding ID, e.g., V-38462. In addition to these four basic tags that all tasks have, there are human-friendly tags such as "ssh" or "dod_logon_banner".

A number of prilimary tasks that do things such as enumerate services on the system and check for the existence of various file will _always_ run unless explicitly skipped by using `--skip tags prelim_tasks`.

Some examples of using tags:

    # Only remediate ssh
    ansible-playbook site.yml --tags ssh

    # Don't change SNMP or postfix
    ansible-playbook site.yml --skip-tags postfix,mail,snmp


License
-------

MIT

<span id="fn1">[1](#note1)</span>: A web based STIG viewer is available [here](https://stigviewer.com/stig/red_hat_enterprise_linux_6/). They are not associated in any way with DISA but have provided a useful tool for viewing the STIGs.
