# Build a Kubernetes cluster using k3s using Ansible


## Preparing the Raspberry Pis

1. Install Raspberry Pi OS Lite(64-bit) on the SD cards
2. Set the hostname and the wifinetwork details
3. Prepare a ssh key and copy the key to the nodes
4. User should be able to ssh into the nodes using the ssh key



## K3s Ansible Playbook

Build a Kubernetes cluster using Ansible with k3s. The goal is easily install a Kubernetes cluster on machines running:

- [X] Debian
- [X] Ubuntu
- [X] CentOS

on processor architecture:

- [X] x64
- [X] arm64
- [X] armhf

## System requirements

Deployment environment must have Ansible 2.4.0+
Master and nodes must have passwordless SSH access

## Usage

- Update the host.ini file in `inventory/nord-pi-cluster` directory.

```bash
[master]
192.16.35.12

[node]
192.16.35.[10:11]

[k3s_cluster:children]
master
node
```

- Also edit `inventory/nord-pi-cluster/group_vars/all.yml` to match your environment.

```bash
---
k3s_version: v1.22.3+k3s1
ansible_user: prasanth
systemd_dir: /etc/systemd/system
master_ip: "{{ hostvars[groups['master'][0]]['ansible_host'] | default(groups['master'][0]) }}"
extra_server_args: ""
extra_agent_args: ""
```

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/my-cluster/hosts.ini
```

## Kubeconfig

To get access to your **Kubernetes** cluster just

```bash
scp userß@master_ip:~/.kube/config ~/.kube/config
```
