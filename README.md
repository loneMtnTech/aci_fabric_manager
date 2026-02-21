# 🚀 Cisco ACI Fabric Manager

[![Ansible Galaxy](https://img.shields.io/badge/ansible.galaxy-lonemtntech.aci__fabric__manager-blue.svg)](https://galaxy.ansible.com/lonemtntech/aci_fabric_manager)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-green.svg)](https://www.gnu.org/licenses/gpl-3.0.html)

A powerful, opinionated Ansible collection designed to streamline and standardize the deployment of Cisco ACI Fabric Access Policies.

---

## 📖 Table of Contents
- [🌟 Overview](#-overview)
- [✨ Key Features](#-key-features)
- [📋 Requirements](#-requirements)
- [🚀 Quick Start](#-quick-start)
- [⚙️ Variables](#️-variables)
- [🛠️ Configuration & YAML Anchors](#️-configuration--yaml-anchors)
- [🏗️ Role Breakdown](#️-role-breakdown)
- [📚 Examples](#-examples)

---

## 🌟 Overview
The `aci_fabric_manager` collection provides a robust framework to achieve a uniform and rapidly deployed base fabric. It is built for network engineers who want to leverage best practices and automation to maintain a consistent Cisco ACI environment.

Whether you are setting up a new fabric or managing an existing one, this collection simplifies complex tasks into manageable, reusable roles.

---

## ✨ Key Features
- **🏗️ Automated Foundations:** Seamlessly create VLAN pools, Domains, and AEPs with [domains_pools_aeps](./roles/domains_pools_aeps/).
- **🔌 Profile Management:** Automatically generate Switch, Interface, and FEX profiles based on node ranges with [switch_profiles](./roles/switch_profiles/).
- **🛠️ Policy Enforcement:** Standardize Interface policies and groups across your fabric with [interface_policies](./roles/interface_policies/).
- **🌐 Global Optimization:** Enable global best practices (MCP, Rogue EP detection, etc.) with [global_settings](./roles/global_settings/).
- **📡 Core Services:** Quickly configure DNS, NTP, and BGP route reflectors with [fabric_basics](./roles/fabric_basics/).
- **🔢 Switch Provisioning:** Pre-stage and manage switch IDs and serial numbers efficiently with [switch_provision](./roles/switch_provision/).
- **🔐 Secure Access:** Configure RADIUS authentication for the fabric with [AAA](./roles/AAA/).

---

## 📋 Requirements
To ensure compatibility and performance, the following versions are required:

- **Ansible:** v13.0 or later
- **Python:** 3.12 or later
- **Cisco ACI Collection:** `cisco.aci` >= 2.13.0
- **Authentication:** ACI user with certificate-based authentication enabled.
  - [Cisco ACI Signature-based Authentication Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_aci.html#signature-based-authentication-using-certificates)

---

## 🚀 Quick Start

### 1. Install the Collection
Install via Ansible Galaxy:
```bash
ansible-galaxy collection install lonemtntech.aci_fabric_manager --force
```

### 2. Create Your Playbook
Create a file named `deploy_fabric.yml`:
```yaml
- hosts: apic
  collections:
    - lonemtntech.aci_fabric_manager
  gather_facts: no
  vars_prompt:
    - name: intent
      prompt: "Desired Action (add/remove)"
      default: add
      private: no
    - name: varprompt_private_key
      prompt: "Private Key Filename (e.g., admin.key)"
      private: no
  roles:
    - interface_policies
    - domains_pools_aeps
    - switch_profiles
    - global_settings
    - fabric_basics
    - switch_provision
```

### 3. Run the Playbook
```bash
ansible-playbook -i inventory.ini deploy_fabric.yml
```

---

## ⚙️ Variables

### Common Variables
| Variable | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `intent` | string | `add` | Controls if infrastructure is deployed (`add`) or deleted (`remove`). |
| `varprompt_private_key` | string | - | Path/filename of the authentication certificate (e.g., `admin.key`). |
| `apic_proxy` | string | `no` | Proxy configuration for APIC access. |
| `apic_validate_certs` | string | `no` | Whether to validate APIC SSL certificates. |
| `apic_use_ssl` | string | `yes` | Whether to use SSL for APIC connections. |

> **Note:** Each role has its own set of required variables. Please refer to the `defaults/main.yml` file within each role for detailed configuration.

---

## 🛠️ Configuration & YAML Anchors

The easiest way to customize your deployment is to create a [host_vars folder](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#organizing-host-and-group-variables) and override the default variables.

YAML anchors are frequently used to establish relationships between objects in variable files. For example, a domain can reference a VLAN pool using an anchor.

### Example: Mapping a VLAN Pool to a Domain
```yaml
vlanPools:
  - &prodVlanPool
    name: prod
    alloc: static
    vlans:
      - blockStart: 1000
        blockEnd: 1099

domains:
  - &prodDomain
    name: prod
    domain_type: phys
    vlan:
      *prodVlanPool
```

---

## 🏗️ Role Breakdown

Each role in this collection is designed for a specific part of the ACI fabric configuration:

- **[AAA](./roles/AAA/)**: Manages Authentication, Authorization, and Accounting (RADIUS support).
- **[domains_pools_aeps](./roles/domains_pools_aeps/)**: Configures the relationship between VLAN pools, physical/virtual domains, and AEPs.
- **[fabric_basics](./roles/fabric_basics/)**: Sets up core fabric services like DNS, NTP, and BGP.
- **[global_settings](./roles/global_settings/)**: Implements best-practice global fabric configurations.
- **[interface_policies](./roles/interface_policies/)**: Manages reusable interface policy groups (CDP, LLDP, MCP, etc.).
- **[switch_profiles](./roles/switch_profiles/)**: Automates the creation of leaf/spine profiles and vPC peering.
- **[switch_provision](./roles/switch_provision/)**: Simplifies the registration of new switches into the fabric.

---

## 📚 Examples
For detailed examples and visual concepts of the fabric deployment, please visit the [Examples Directory](./examples/).

---

## 📄 License
This collection is licensed under the [GNU General Public License v3.0](./LICENSE).
