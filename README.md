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
      ping:					  # De naam van de module die we gebruiken

```

Telkens dat een playbook wort uitgevoerd kunnen we met ```gather_facts``` ansible heel wat info laten verzamelen over de hosts. Deze info kan later opgevraagd worden via ```ansible_facts```

### Tasks
* Geef alle tasks een naam!
* Modules zijn de ***commands*** die ansible kent
* Ansible heeft een uitstekende docbase!
* https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html

### Playbooks runnen

```
ansible-playbook example-playbook.yml -i inventory
ansible-playbook example-playbook.yml -i inventory --limit tsvm1
```

### Opdracht

* Maak een nieuwe playbook waarmee het programma ```tree``` wordt geinstalleerd.
	* https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html
* Run je playbook eerst met ```--limit tsvm1```
* Run je playbook nadien zonder ```--limit``` zodat alle hosts getarget worden.
* Wat zie je in de output?
