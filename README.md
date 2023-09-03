# Ansible Workshop
This repository contains the information and code for an Ansible Workshop. The parts are split up between multiple bbranches

## Roles

Roles in Ansible zijn verzamelingen van samenhorende taken, files, templates, variables...

Gelijkaardig aan playbooks dus, maar met wat bijkomende functionaliteiten.

### Role gebruiken in playbook

```yaml
- name: playbook that uses a role
  hosts: somehosts
  gather_facts: yes
  become: yes
  roles:
  	- example-role
```

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
	  tasks/
	    main.yml
	  defaults
	    main.yml
	  files/
	    somefile
	  templates/
	    sometemplate.j2
	  handlers/
	    main.yml
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
# bovenstaande is een defaultwaarde van de role, 
#    met lagere precedence dan group & host vars.
# heel handig om group_vars en host_vars compact te houden,
#    en het gebruik van roles flexibel te maken.
example_role__testvar: "dit is de default waarde van testvar"
```

### Templates
Templates (zie ```template``` module) zijn files met variabelen in. 

Deze worden bij het runnen van de role ingevuld door ansible, op basis van de beschikbare variabelen.

```jinja2
de inhoud van deze file komt terecht op {{ ansible_host }}
{% if extra_berichtje is defined %}
vervolgens ook nog volgende boodschap: {{ extra_berichtje }}
{% endif %}
```

### Handlers
Handlers zijn speciale tasks die enkel uitgevoerd worden indien ze opgeroepen worden vanuit de main tasks (met keyword ```notify```).

Deze handlers worden pas uitgevoerd nadat alle andere taken zijn uitgevoerd.

Ze worden maar 1x uitgevoerd, ook al worden ze meerdere keren opgeroepen.

Voornaamste use-case: restarten/reloaden van services wanneer bepaalde configuratiefiles zijn aangepast.

#### Voorbeeld:

In onderstaande setup wordt nginx enkel gereload indien er effectief iets is veranderd aan de config.

* tasks/main.yml:
```yaml
- name: update nginx configuration
  lineinfile:
    path: "/etc/nginx/nginx.conf"
    line: "user {{ webserver__user }}"
    state: present
  notify: reload nginx
```

* handlers/main.yml:
```yaml
- name: reload nginx
  service:
    name: nginx
    state: reloaded
```

## Opdracht 1

* Maak een role die een webserver naar keuze (apache/httpd, nginx...) installeert op tsvm2 en tsvm3.
	* Bepaal in host_vars op welke poort de webserver moet luisteren voor http:
		* poort 9082 voor tsvm2
		* defaultwaarde (defaults/main.yml) 9080
	* Installeer een eenvoudige index.html die als paginatitel de hostnaam van de webserver bevat
		* hint: gebruik een template
	* Zorg ervoor dat de webserver aangezet wordt (service gestart dus)
		* subsequent runs zonder changes mogen de service niet aanraken
* Controleer & Experimenteer!

## Opdracht 2

* Maak een role die een loadbalancer (haproxy bijvoorbeeld) installeert op tsvm1
	* configureer tsvm2 en tsvm3 als backends
