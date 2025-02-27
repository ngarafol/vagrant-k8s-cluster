# Vagrant Kubernetes

Table of Content
  * [Requirements](#requirements)
  * [Setup](#setup)
  * [Kubernetes version](#kubernetes-version)
  * [Starting](#starting)
  * [Examples](#examples)
  * [Credits](#credits)

A small playground to experiment or play with Kubernetes on multiple Vagrant Ubuntu `ubuntu/bionic64` instances. So do not use this as a base for production like deployments (Kubespray for example).

## Requirements

Please make sure the following is installed before using this repo:

* Ansible;
* Vagrant;
* VirtualBox;
* Motivation!

## Setup

Default the following is started/installed:

* 1 Control node;
* 2 Worker nodes;

Control node has 2 CPU and 1GB of RAM. Each worker node has 1 CPU's configured with each 1GB of RAM. You can change this to your needs by updating the `Vagrantfile`.

## Kubernetes version

Kubernetes version 1.25.3 (CKA currently is 1.25) which can be changed in the `Vagrantfile` by looking in the top of the file for the line that starts with: `K8S_VERSION`. You can set that to a more recent version of Kubernetes before you start everything.

```ruby
    IMAGE_NAME = "ubuntu/jammy64"
    K8S_VERSION = "1.25.3"
    N = 2
```

## Starting

Once you are ready, run the following to start everything:

```sh
vagrant up
```
** If virtualbox is complaining about ip address range used, you could create vbox networks file and fix it, or change ip range to allowed by vbox **
```
$ cat /etc/vbox/networks.conf
* 0.0.0.0/0 ::/0
```

Once everything is booted, use the following command to logon to the control node:

```sh
vagrant ssh control
```

You should be able to run `kubectl get nodes` now.

## Examples

In the `examples` [directory](examples/readme.md), you can find some example questions that you can use to get familiar with Kubernetes.

## Credits

Combination of code https://graspingtech.com/create-kubernetes-cluster/ and some custom things.
Containerd migration inspired by https://github.com/justmeandopensource/kubernetes/blob/master/vagrant-provisioning/bootstrap.sh
