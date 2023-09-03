# Ansible Workshop
This repository contains the information and code for an Ansible Workshop. The parts are split up between multiple bbranches

## Roles

Roles in Ansible zijn verzamelingen van samenhorende taken, files, templates, variables...

Gelijkaardig aan playbooks dus, maar met wat bijkomende functionaliteiten.

### Directory structuur

```
ansible/
  example-playbook.yml
  group_vars/
    ...
  host_vars/
    ...
  inventory
  roles/
    example-role/
	  tasks/main.yml
	  defaults/main.yml
	  files/
	    somefile
	  templates/
	    sometemplate.j2
	  handlers/main.yml
```

### example-role/tasks/main.yml
```yaml
---
- name: this is a task 
  debug:
    msg: "this is a task"

- name: this is a task using a variable
  debug:
    var: example_role__testvar
```

### example-role/defaults/main.yml
```yaml
---
# bovenstaande is een defaultwaarde van de role, met lagere precedence dan group & host vars.
# heel handig om group_vars en host_vars compact te houden, en het gebruik van roles flexibel te maken.
example_role__testvar: "dit is de default waarde van testvar"
```

### example-role/templates/sometemplate.j2
Templates (zie ```template``` module) zijn files met variabelen in. 

Deze worden bij het runnen van de role ingevuld door ansible, op basis van de beschikbare variabelen.

### Role gebruiken in playbook

```yaml
- name: playbook that uses a role
  hosts: webserver
  gather_facts: yes
  become: yes
  roles:
  	- webserver
```
