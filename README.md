# Home Lab

Documentation for my personal home lab: a segmented Ubiquiti network, a Proxmox virtualization host, a Synology NAS, and a Raspberry Pi providing network-wide ad blocking, DNS, and DHCP. The lab doubles as a study environment for CCNP, using EVE-NG running on Proxmox. and outcome of the project.

> [!NOTE]
> Technical details have been generalized to protect the security of my internal network. Each entry still documents the scope 

## Table of Contents

- [Overview](#overview)
- [Network Topology](#network-topology)
- [Hardware](#hardware)
- [Network Design](#network-design)
- [Virtualization](#virtualization)
- [Storage](#storage)
- [Services](#services)
- [CCNP Study Environment](#ccnp-study-environment)
- [Roadmap](#roadmap)

## Overview

| Area | Summary |
|------|---------|
| Routing / switching / wireless | Ubiquiti router, switch, and WAP, EVE-NG Study Labs |
| Power | Ubiquiti PDU |
| Virtualization | Proxmox VE with multiple VM instances |
| Network labbing | EVE-NG running on Proxmox |
| Storage | Synology NAS |
| DNS / DHCP / ad blocking | Raspberry Pi |
| VLANS | LAB, FAMILY, IOT, MANAGEMENT |

## Network Topology

The lab sits behind its own Ubiquiti router. A second router, also fed by the ISP modem, serves the family network, IoT devices, and other appliances, keeping them fully separate from the lab with only two devices from the family network allowed to talk to lab network through management VLAN.

<img width="2720" height="2360" alt="home_lab_network_topology" src="https://github.com/user-attachments/assets/23857dac-ae15-4219-8f81-d6e7d93dec64" />

## Hardware

| Device | Role | Notes |
|--------|------|-------|
| Ubiquiti router | Lab gateway, inter-network routing and firewalling | Connects upstream to the ISP modem |
| Ubiquiti switch | Wired backbone for the lab | |
| Ubiquiti WAP | Wireless access | |
| Ubiquiti PDU | Managed power distribution for lab gear | |
| Raspberry Pi | DNS, DHCP, network-wide ad blocking ||
| Proxmox host | Hypervisor for VMs and EVE-NG | EVE-NG is used for CCNP Labbing |
| Synology NAS | Network storage ||
| Second router | Family, IoT, and appliance network | Separate from the lab, downstream of the ISP modem |
| ISP modem | Internet uplink | |

## Network Design

### Segmentation

Networks are segmented so that lab, family, and IoT devices cannot reach each other unless explicitly allowed.

- **Lab network:** behind the Ubiquiti router. Hosts Proxmox, the NAS, the Raspberry Pi, and lab VMs.
- **Family / IoT / appliance network:** behind the second router, isolated from the lab at the router level.

### DNS and DHCP

The Raspberry Pi is the DNS and DHCP server for the lab network and runs network-wide ad blocking. All lab clients receive the Pi as their resolver via DHCP.

## Virtualization

Proxmox VE hosts multiple virtual machine instances, including an EVE-NG VM used for network labbing.

| VM / Service | Purpose |
|--------------|---------|
| EVE-NG | CCNP lab topologies | 
| VaultWarden | Password Manager |
| Guacamole | Used as the jumpbox for all proxmox VMs |
| Linux Server | Hosting internal dashboard for monitoring Lab environment |

## Storage

The Synology NAS provides network storage for the lab.


## Services

| Service | Host | Purpose |
|---------|------|---------|
| Ad blocking | Raspberry Pi | Network-wide DNS filtering |
| DNS | Raspberry Pi | Name resolution for the lab |
| DHCP | Raspberry Pi | Address assignment for the lab |
| EVE-NG | Proxmox VM | Network emulation for CCNP study |
| File storage | Synology NAS | Shared storage and backups |

## CCNP Study Environment

EVE-NG runs as a VM on Proxmox and is used to build and test CCNP ENCOR topologies (routing, switching, and related enterprise networking labs).

- Lab traffic stays on its own segment and is isolated from the production lab and family networks.
- Topology files and lab notes can be stored under `docs/labs/`.

## Roadmap

- [ ] Add CCNP lab topologies and notes
