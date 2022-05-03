# Amazon Linux 2 optimized for EKS

## Introduction
------------

This ansible content will configure an Amazon Linux 2 optimized for EKS to be GSA complaint.

This code is based on the GSA [GSA Amazon Linux Security Benchmark](https://docs.google.com/spreadsheets/d/1Bd60R99Uij2bkvDdGONL7vyq2t_-ltXukVLJ9DhfB7k/edit#gid=884169645).

Code for the GSA EKS Benchmark will be coming soon.

### Pre-Hardened Amazon Machine Images
ISE provides a maintained and hardened AWS EKS AMI. More information about usage can be found [here](https://cautious-happiness-f7ecfe89.pages.github.io/techdoc_hardenedami_introduction.html)

### Important Information

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.

Role Variables
--------------
There are many role variables defined in defaults/main.yml.

##### The current default configuration will:
* Enable IPv6 settings
* Enable iptables
* Enable auditing with rsyslog.
* Lock users inactive for over 30 days.

##### The configuration will not:
* Install and configure AIDE
* Install and configure NTP
* Configure the /etc/group wheel configurations

Other settings and services are listed. Please review to ensure they meet your organizational requirements.

Note, a subset of controls were removed due to operational impact or organizational dependent variables. Those are listed [here](https://docs.google.com/spreadsheets/d/1nNB-irY7qALa-3K0dTL8P5sTQNMA0DhzsNXW-x9aAxE/edit#gid=0).


### Dependencies
Ansible >= 2.7

### Example Playbook

```
---
- name: Harden Server
  hosts: all
  become: yes

  roles:
    - ansible-os-amazon-linux2-eks
```
### How to test locally

```
ansible-playbook playbook.yml --connection=local
```

### License

BSD.
