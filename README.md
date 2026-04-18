# Ansible SSH Key Manager

Manage and distribute SSH public keys across multiple servers using Ansible.

---

## Overview

This project provides a simple and idempotent way to manage SSH `authorized_keys` across multiple hosts. It ensures consistent access control and simplifies SSH key distribution in multi-server environments.

---

## Features

* Add and remove SSH public keys
* Idempotent execution (safe to run multiple times)
* Supports multiple hosts via inventory
* Clean and minimal structure

---

## Project Structure

```text
.
├── inventory.yml
├── manage_ssh_keys.yml
└── vars/
    └── ssh_keys.yml
```

---

## Requirements

* Ansible 2.9+
* SSH access to target servers
* Python installed on target hosts

---

## Usage

Run the playbook:

```bash
ansible-playbook -i inventory.yml manage_ssh_keys.yml
```

---

## Configuration

### Inventory

Define your target hosts:

```yaml
all:
  children:
    servers:
      hosts:
        server1:
          ansible_host: 1.2.3.4
          ansible_user: fedora
```

---

### SSH Keys

Edit `vars/ssh_keys.yml`:

```yaml
ssh_keys:
  - name: "test-key"
    key: "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... your_key_here"
    state: present
```

---

## How It Works

* Ensures `.ssh` directory exists for the target user
* Manages `authorized_keys`
* Adds or removes keys based on desired state

---

## Idempotency

This playbook is idempotent. Running it multiple times will not introduce changes if the desired state is already achieved.

Example:

```text
changed=0
```

---

## Using SSH Config (Optional)

You can also rely on your local SSH configuration (`~/.ssh/config`) and keep the Ansible inventory minimal by referencing only host aliases.

### YAML Inventory Example

```yaml
all:
  hosts:
    my-server:
```

Example SSH config:

```bash
Host my-server
    HostName 1.2.3.4
    User fedora
    IdentityFile ~/.ssh/id_ed25519
```

Ansible will use your SSH config for connection details, so you don't need to redefine `ansible_host`, `ansible_user`, or SSH key paths in the inventory.

This keeps your inventory clean and avoids duplication.

---

## Security Notes

* Only store **public keys** in this repository
* Never commit private keys
* Use Ansible Vault if you later extend to sensitive data

---

## Future Improvements

* Convert to Ansible role structure
* Add CI (ansible-lint, syntax checks)
* Support group-based key management
* Dynamic inventory integration
