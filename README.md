# Ansible Workshop
This repository contains the information and code for an Ansible Workshop. The parts are split up between multiple bbranches

## Ansible Playbooks

Playbooks zijn verzamelingen van tasks (of roles, maar daarover later meer) die kunnen worden gerunt.

bv:
```yaml
---
- name: example playbook
  hosts: all                  #pattern van targets voor dit playbook
  gather_facts: yes           #informatie verzamelen over de hosts?
  become: yes                 #root worden op de target machines?
  tasks:                      # Lijstje van tasks, elke listentry begint met een "-"
    - name: ping de machines
      ping:

```
