# Build a Kubernetes cluster using Rancher Kubernetes Engine 2 (RKE2) via Ansible

## RKE2 Ansible Playbook

Build a Kubernetes cluster using Ansible with rke2. The goal is easily install a Kubernetes cluster on machines running:

- [X] Ubuntu
- [X] CentOS 8

on processor architecture:

- [X] x64

## System requirements

Deployment environment must have Ansible 2.14.0+
controllers and workers should have passwordless SSH access

## Usage

First create a new directory based on the `sample` directory within the `inventory` directory:

```bash
cp -R inventory/sample inventory/
```

Second, edit `inventory/hosts.ini` to match the system information gathered above. For example:

```bash
[controller]
192.168.1.10

[worker]
192.168.1.11
192.168.1.12

[balancer]
192.168.1.13

[k8s_cluster:children]
controller
worker
```

If multiple hosts are in the controller group, the playbook will automatically set up rke2 in [HA mode with etcd].

This cluster was designed to use an external NGINX load balancer for the Control Plane API

This requires at least rke2 version `1.24.13+`. The version is configurable by using the `rke2_version` variable.

If needed, you can also edit `inventory/group_vars/all.yml` to match your environment.

### Create Cluster

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/hosts.ini
```

After deployment control plane will be accessible via virtual ip-address which is defined in inventory/group_vars/all.yml as `apiserver_endpoint`

### Remove RKE2 cluster

```bash
ansible-playbook reset.yml -i inventory/hosts.ini
```

>You should also reboot these nodes 

## Kube RKE2 Config

To get access to your **Kubernetes** cluster just

```bash
scp adminuser@controller_ip:~/.kube/config ~/.kube/config
```
