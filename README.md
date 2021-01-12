# Dev cluster with Hashicorp stack and docker

This is an [Ansible](https://www.ansible.com/) playbook to setup a simple development cluster environment where you can run your containerized applications and services.

## Prerequisites

- Ansible
- Debian or Debian based (must have apt as package manager) host somewhere

In order to run the playbook, you need to specify a host in the ansible inventory (i.e. hosts.yml). See in the [Ansible documentation](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html) how to build one. The host should be **Debian** based. I'm using **Debian** on all of my servers, so this playbook is developed against it.

## What's inside the playbook
The playbook is divided into several 'plays' or groups of tasks, dedicated to each segment of the cluster.
Here's a list of the plays that this playbook runs:

 - init: updates the operating system and installs *dnsmasq* and *unzip*
 - docker: installs docker runtime
 - consul: installs and setup the Hashicorp's Consul
 - dnsmasq: setup dnsmasq as forward DNS proxy for Consul
 - nomad: installs and setup Hashicorp's Nomad

## How to run
Simply execute the following line in the command line:

```
    $: ansible-playbook -i hosts.yml playbook.yml
```

Once finished, your cluster is up and running.

## Remarks

 - **The cluster does not comply with the recommended reference architecture from Hashicorp**. See https://learn.hashicorp.com/tutorials/nomad/production-reference-architecture-vm-with-consul to know more about what is recommended.
 - The cluster runs on a **single host in client and server mode**. You can easily add more nodes to run clients and other nodes to run the servers.
 - The playbook bootstraps Nomad with ACL enabled. The management token and single client token are available in the host executing the playbook under **/opt/nomad.out** and **/opt/dev.out**. More on Nomad ACL here: https://learn.hashicorp.com/tutorials/nomad/access-control.
