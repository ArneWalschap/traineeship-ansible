# Ansible Workshop
This repository contains the information and code for an Ansible Workshop. The parts are split up between multiple bbranches

## Variables

variables kunnen op heel wat niveaus worden gedeclareerd en overschreven. 

De variable precedence kan je raadplagen op:

https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable

### group_vars

Variables kunnen toegewezen worden aan groepen van hosts.

Ansible kijkt hiervoor automatisch op de volgende manier in je directory:

```
ansible/
	inventoryfile
	playbook.yml
	group_vars/
		vagrant/
			main.yml
```

in dit voorbeeld zullen alle variabelen beschreven in main.yml worden ingelezen voor alle hosts die in de vagrant group zitten)

Ook wanneer je een playbook toepast op ```all``` hosts, zullen group_vars van vagrant worden ingelezen indien de hosts in deze groep zitten in de inventoryfile!

### ansible/group_vars/example/main.yml
```yaml
---
simple_variable_string: "this is a string"
simple_variable_number: 3
list_example_one: ["this", "is", "one way", "to format a list"]
list_example_two:
  - "this"
  - "is"
  - "another way"
  - "to format a list"
dict_example:
  - name: "bob"
    occupation: "bouwer"
  - name: "jan"
    occupation: "mosselman"
  - name: "dopper"
  - name: "sinterklaas"
    occupation: "kindervriend"
```

### host_vars

We kunnen variabelen ook in host_vars beschrijven voor specifieke hosts.

Indien een variabele in group_vars en host_vars voorkomt, zal deze in host_vars die van group_vars overschrijven.

Qua directory werkt dit op dezelfde manier als group_vars.


```
ansible/
	group_vars/
		...
	host_vars/
		tsvm1/
			main.yml
```

### ansible/host_vars/tsvm1/main.yml
```yaml
---
simple_variable_string: "deze string heeft de vorige van group_vars overschreven"
```

## Using variables in tasks

```yaml
---
- name: example playbook
  hosts: all                  #pattern van targets voor dit playbook
  gather_facts: yes           #informatie verzamelen over de hosts?
  become: yes                 #root worden op de target machines?
  tasks:
    - name: simple variable use
      debug:
        msg: "the string contained in simple_variable_string is: {{ simple_variable_string }}"

    - name: list variable use
      debug:
        msg: "the list in list_example_one is: {{ list_example_one }}"

    - name: dict variable without message
      debug:
        var: dict_example

    - name: loop example
      debug:
        msg: "{{ item.name }} gaat naar de bakker."
      with_items: "{{ dict_example }}"

    - name: default fallback example (to make variables optional)
      debug:
        msg: "{{ item.name }} is {{ item.occupation | default('werkloos') }}"
      with_items: "{{ dict_example }}"
```

## Opdracht
### Users
* Maak een playbook dat een lijstje van users aanmaakt (gebruik de ```user``` module in ansible).
* Definieer het lijstje met users in group_vars/vagrant/main.yml
* Overschrijf de variabele voor tsvm1 in host_vars/tsvm1/main.yml

users in group_vars/vagrant/main.yml:
```yaml
---
opdracht_users:
  - "bob"
  - "alice"
```

users in host_vars/tsvm1/main.yml:
```yaml
---
opdracht_users:
  - "babette"
  - "Emily"
```

#### vervolg
* Refactor voorgaande zodanig dat je optioneel ook een comment kan toevoegen aan de users (te definieren in de host of group variables)

eg:
```yaml
---
opdracht_users:
  - name: "bob"
    comment: "dit is bob"
  - name: "alice"
```
