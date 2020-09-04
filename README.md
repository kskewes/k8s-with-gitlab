# Home lab

Multi-arch Kubernetes cluster for experimenting with various tools.

## Prerequisites

**Hardware (or any combination):**
- 6x Rock64 SBC's running Ubuntu and Kubernetes 
   - 3x Masters with SSD disk
   - 3x Workers with eMMC disk
   - [OS image on media](https://github.com/ayufan-rock64/linux-build/releases) - latest bionic-minimal-rock64
- 1x AMD64 Host running KVM
   - (will create) 3x Workers provisioned with Terraform

**Software:**
- [Ansible](https://docs.ansible.com)
- [Terraform](https://terraform.io) 
- [terraform-provider-libvirt](https://github.com/dmacvicar/terraform-provider-libvirt/)

**Networking:**

| Item | Range |
| :--- | :--- |
| Network | 192.168.2.0/24 |
| Gateway | 192.168.2.1 |
| Ingress | 192.168.3.0/28 | 
| ARM64 K8s Masters | 192.168.2.30+
| ARM64 K8s Workers | 192.168.2.50+
| AMD64 K8s Workers | 192.168.2.60+

## Diagnostics

- [Rock64 Serial](https://forum.pine64.org/showthread.php?tid=5029) - From right, skip 2 pins, ground (6), TX (8), RX (10)
- `sudo  minicom  -s  -D  /dev/ttyUSB0  -b  1500000  --color=on`

## Getting Started

0. Git clone this repo and submodules ([kubespray](https://github.com/kubernetes-sigs/kubespray)) - `git clone --recurse-submodules ...`
1. Prepare hardware
2. Provison Terraform nodes
   - TODO: `make deploy-terraform`
3. Configure Ansible
   - Edit Ansible inventory, variables as required
   - `make deploy-ansible-site`
4. Ansible configure Kubernetes cluster
   - Edit Ansible inventory, variables as required
   - `make deploy-kubespray`
5. Retrieve Kubernetes cluster-admin credentials
   - `make retrive-kubespray-kubeconfig`
6. Deploy Kubernetes applications
   - Edit as required
   - TODO: `make deploy-kubernetes`
   - `kubectl apply -f kubernetes/`
7. BGP Peer EdgeRouter and Metallb
   - `make deploy-ansible-edgeos`
