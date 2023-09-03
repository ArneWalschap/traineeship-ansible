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

### Idempotency

Als alles goed ging in vorige opdracht heb je kunnen zien dat ansible enkel iets "changed" wanneer nog niet voldaan is aan de gewenste state.

Dit is het belangrijkste verschil tussen Ansible en een eenvoudig bash-scriptje.

Enorm belangrijk om ervoor te zorgen dat alle ansiblecode ***idempotent*** is, en dus keer op keer kan uitgevoerd worden zonder iets stuk te maken.

Bijvoorbeeld: Je wilt een playbook maken dat ervoor zorgt dat er een bepaalde configlijn aanwezig is in de sudoers file. Je moet hiervoor de juiste modules gebruiken, en niet gewoon een lijn toevoegen aan het einde van het bestand, want als je dat laatste doet zou hij bij elke herhaalde run opnieuw een lijn toevoegen.

### Volgende stappen
```git checkout ansible-vars```
