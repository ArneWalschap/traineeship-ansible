# Ansible Workshop
This repository contains the information and code for an Ansible Workshop. The parts are split up between multiple bbranches

## Ansible ad-hoc

We willen ansible gebruiken om eenvoudig een commando uit te voeren op meerdere machines in onze omgeving.

Ad-hoc commando's zijn nuttig om informatie te vragen van meerdere machines (bv hoeveelheid memory, uptime, ip adressen...)

```bash
ansible PATTERN -m MODULE_NAME -a MODULE_ARGS -i INVENTORYFILE
```

bv:
```
ansible tsvm1,tsvm2 -m ping -i inventory
ansible all -m shell -a "uptime" -i inventory
ansible all -m shell -a "cat /etc/hostname" -i inventory
...
```

### Inventory
De inventory file bevat informatie over de hosts die bereikbaar zijn voor ansible.
Het is ook mogelijk om de hosts in groups te plaatsen, en om variabelen te definieren per groep of per host.
Inventory files kunnen zowel in .ini als in .yml formats geschreven worden, maar .ini wordt het meest gebruikt.

Voorbeeld:

```ini
[vagrant]
tsvm1
tsvm2
tsvm3
```

### Variabelen
standaard zal ansible proberen om te ssh'en naar de hosts met als user je huidige gebruiker, en als ssh-keys de keys die in je ssh-agent zitten.
Om ervoor te zorgen dat we kunnen verbinden zullen we dus enkele variabelen moeten (her)configureren.

```ini
[vagrant]
tsvm1 ansible_ssh_private_key_file=~/ansible-workshop/vagrant/.vagrant/machines/tsvm1/virtualbox/private_key
tsvm2 ansible_ssh_private_key_file=~/ansible-workshop/vagrant/.vagrant/machines/tsvm2/virtualbox/private_key
tsvm3 ansible_ssh_private_key_file=~/ansible-workshop/vagrant/.vagrant/machines/tsvm3/virtualbox/private_key

[vagrant:vars]
ansible_user=vagrant
ansible_host_key_checking=false
```

### Uitvoeren

Als alles goed is, zou je nu ad-hoc commando's moeten kunnen uitvoeren.
Probeer de commando's die hierboven beschreven staan eens uit, en experimenteer!

