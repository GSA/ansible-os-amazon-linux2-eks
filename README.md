#Amazon Linux 2 optimized for EKS

## Introduction
------------

This ansible content will configure an Amazon Linux 2 optimized for EKS to be GSA complaint.

This code is based on the GSA  AWS Elastic Kubernetes Service Benchmark.

### Items of Note

####Amazon Linux 2 and SE Linux
By default SElinux is disabled via grub in amazon linux.

The tasks in Section 1.6 of this role will enable SELinux. Alternatively, it can be manually enabled by following these steps:

edit ```/boot/grub/menu.lst```
Modifiy ```selinux=0  to  selinux=1```
        touch /etc/selinux/config'''

Also install the following package to allow the ansible SElinux module to function on the host.
```yum install libselinux-python```

A reboot will be necessary for the changes to take effect.


### Example Playbook
An example playbook which uses this role is as follows:


- hosts: localhost
  connection: local
  gather_facts: true
  become: yes

  roles:
    - ansible-cis-amazon-linux-2
```

A more advanced example, which includes modifications to the default values used, as well as the exclusion of some items in the benchmark which are considered unnecessary for a fictional environment, is as follows:

```
---

- hosts: localhost
  connection: local
  gather_facts: true
  become: yes

  vars:
    cis_level_1_exclusions:
      - 5.4.4
      - 3.4.2
      - 3.4.3
      - 6.2.13   
    cis_pass_max_days: 45
    cis_umask_default: 027

  roles:
    - { role: ansible-cis-amazon-linux-2,
        in_vm: true,
        cis_hosts_allowed_ip_range: '10.',
        cis_umask_default: "027",
        full_upgrade: true,
        cis_enable_rsyslog: true,
        cis_level_1_exclusions: [ 3.5.1.1, 5.3.4 ],
        cis_level_2_exclusions: [ ]
        cis_rsyslog_server: "myserver.com:1555"
      }

```
either one of `in_vm` or `in_vagrant` must be set (to any value) according to your target/testing environment.
Note that the use of ```become: yes``` is required as 99% of tasks require privileged access to execute.

Role Variables
--------------

See ```defaults/main.yml``` for variables which can be overriden according to preference.

Options
-------

Tags (and combinations thereof) can be used to run a particular level of the CIS standard, a section, or an individual recommendation. For example:

* Run only Level 1 tasks

```
ansible-playbook playbook.yml -t level-1
```

* Run only Section 3 tasks

```
ansible-playbook playbook.yml -t section-3
```

* Run tasks 1.3.1 and 2.1.10 only

```
ansible-playbook playbook.yml -t 1.3.1,2.1.10
```

* Run scored tasks only

```
ansible-playbook playbook.yml -t scored
```

Limitations
-----------

At present, only the Level 1 items of the benchmark are implemented. Level 2 items will be added as time permits.

The following checks have not been implemented:

* 3.4.*. Firewall rulesets are environment specific.
* 3.5.*. Firewall rulesets are environment specific.
* 4.2.1.2. The determination of what should be logged and the destination of messages is environment specific.
* 4.2.2.2. The determination of what should be logged and the destination of messages is environment specific.
* 4.2.2.3. Inline editing of syslog-ng configuration file is considered too imprecise and is best solved by a supplied configuration file which addresses this and other related requirements.
* 4.2.2.4. Inline editing of syslog-ng configuration file is considered too imprecise and is best solved by a supplied configuration file which addresses this and other related requirements.
* 4.2.2.5. Inline editing of syslog-ng configuration file is considered too imprecise and is best solved by a supplied configuration file which addresses this and other related requirements.
* 4.3. The configuration of logrotate is site-specific.
* 5.3.2. Multi-line editing of pam configuration files is considered too imprecise and dangerous, and is best solved by a supplied configuration file which addresses this and other related requirements.
* 5.3.3. Multi-line editing of pam configuration files is considered too imprecise and dangerous, and is best solved by a supplied configuration file which addresses this and other related requirements.
* 5.5. Not implemented, because of potential impact to su -, and due to not knowing which consoles are in physically secure locations within AWS/site premises.

Compatibility
-------------

This role should be compatible with version ```2.6.15``` of Ansible.
It should work with any point release within Ansible 2.6 major release, and there's a high likelihood of it working fine under Ansible 2.5, 2.7.

Vagrant version ```2.2.4``` was used during the development & testing of Amazon Linux 2 revision of the role.



License
-------

BSD.
