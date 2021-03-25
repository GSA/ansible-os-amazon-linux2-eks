# Amazon Linux 2 optimized for EKS

## Introduction
------------

This ansible content will configure an Amazon Linux 2 optimized for EKS to be GSA complaint.

This code is based on the GSA  AWS Elastic Kubernetes Service Benchmark.

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

Note, a subset of controls were removed due to operational impact or organizational dependent variables. Those are listed [here](https://github.com/GSA/ansible-os-amazon-linux2-eks/blob/master/misc/compliance_settings.md).


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
### CircleCI Intergration

This repository has been updated to optionally utilize Continuous Intergration with CircleCI and tests the ansbile tasks against a privledged CentOS-7 Container.  A low number of tasks are incompatiable when ran against a container vs a vm or bare-metal and have ignore_errors turned on.

##### Using CircleCI:
* Fork this repository or create a branch
* Sign up for an account and follow the getting started guide at https://circleci.com/docs/2.0/first-steps/#section=getting-started
* Add the repository to your projects and click start building. https://circleci.com/docs/2.0/project-build/#section=getting-started
* New Commits will trigger the CircleCI build and run the playbook.yml and the result will pass or fail.

### License

BSD.
