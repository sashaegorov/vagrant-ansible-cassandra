# Introduction

Easily provision Cassandra 2 cluster using Ansible with Vagrant & Virtualbox.

## Prerequisites

* Versions of [Virtualbox](https://www.virtualbox.org/) _v.5.0.14_ and [Vagrant](https://www.vagrantup.com/downloads) _v.1.8.1_ are known to work
* [Ansible](http://docs.ansible.com/intro_installation.html) installed on host

## Provisioning

Clone the project and run `vagrant up --parallel` in project directory.

Your cluster will be ready ~~shortly~~ in some time, depending on your Internet connection. The initial boot takes some time as Ansible has to download, install and configure Cassandra on each node. Subsequent reboots are fast.

Nodes will be running on: `192.168.56.1X`.

## Vagrant VM lifecycle

- Show VMs status `vagrant status`
- Connect via SSH node with `vagrant ssh <node>`
- Shutdown whole cluster `vagrant halt`
- Resume all VMs `vagrant up`
- Destroy (requires long re-provisioning): `vagrant destroy [--force]`

## Check Cassandra

- Connect to any node and perform the following `nodetool status`.

# Known issues

- **Sometime after provision finishes, `nodetool status` shows no nodes.** Check Cassandra service status on each node `sudo service cassandra status` and restart it sudo `service cassandra restart`. After restart node should join cluster. In some cases `vagrant reload` also works.
- **Nothing happens.** This configuration runs nodes VMs first and than  provisioner VM. In case if there is no sufficient amount of memory on machine, it will start only one or two. Check VMs status via `vagrant status` and reduce amount of nodes.
- **On Windows Vagrant version 1.8.1 has a bug** which prevents correct folder mapping. Make sure latest Virtualbox version is installed.
- **On Windows hosts connection timeout occures**. Enable GUI and check what's happening.

# Node count

In order to update node number follow this checklist:
- Add them to `ansible/vagrant_inventory` file which responsible for SSH-access
- Edit them to `ansible/vagrant.yml` playbook
- Add new file to `ansible/host_vars`

# TODO

- Generate some file programmatically e.g. `vagrant_inventory`
- Remove workaround for Ansible v.2.0 in future
- ~~Move to Oracle JDK as recommended in Cassandra logs~~

# Credits

Here are code of community contributors who made this configuration possible:

- https://github.com/chbatey/vagrant-cassandra-spark/
- https://github.com/joeljacobson/vagrant-ansible-cassandra
