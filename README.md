# Introduction

Easily provision Cassandra cluster using Ansible with Vagrant & Virtualbox.

## Prerequisites

* [Virtualbox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/downloads) v.1.8
* [Ansible](http://docs.ansible.com/intro_installation.html) installed on host

## Provisioning

Clone the project and run `vagrant up --parallel` in project directory.

Your cluster will be ready shortly depending on your internet connection. The initial boot takes some time as Ansible has to download, install and configure Cassandra on each node. Subsequent reboots are fast.

Nodes will be running on: `192.168.56.1X`.

## Vagrant VM lifecycle

- Show VMs status `vagrant status`
- Connect via SSH node with `vagrant ssh <node>`
- Shutdown whole cluster `vagrant halt`
- Resume all VMs `vagrant up`
- Destroy (requires long re-provisioning): `vagrant destroy [--force]`

## Check Cassandra

Connect to any node and perform the following `nodetool status`.

# TODO

- Remove workaround for Ansible v.2.0
- ~~Move to Oracle JDK as recommended in logs~~

# Credits

Here are code of community contributors who made this configuration possible:

- https://github.com/chbatey/vagrant-cassandra-spark/
- https://github.com/joeljacobson/vagrant-ansible-cassandra
