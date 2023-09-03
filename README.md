# Ansible Workshop
This repository contains the information and code for an Ansible Workshop. The parts are split up between multiple bbranches

## Labo-omgeving: prerequisites

We werken met Vagrant om 3 virtuele machines te lanceren op je lokale machine.


Dit werkt makkelijk, omdat we de machines eenvoudig kunnen "resetten" naar hun initiele status en opnieuw from-scratch een provisioning kunnen testen.

We moeten volgende zaken installeren:
* Ansible
* Vagrant >=2.13
* Virtualbox 7

### Ansible installeren op je laptop

```bash
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt update && sudo apt install ansible
```

### Installatie Virtualbox

* Download .deb package van https://www.virtualbox.org/wiki/Linux_Downloads
* apt install gedownloade package

### Installatie Vagrant

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
```

## Labo-omgeving: opspinnen

Bekijk vagrant/Vagrantfile.

Deze file beschrijft de machines die vagrant moet opspinnen. We zullen 3 VM's (tsvm1,tsvm2,tsvm3) aanmaken.

CD naar vagrant/ in je terminal en voer het commando ```vagrant up``` uit. De eerste keer dat je dit doet moet vagrant de machine-image "ubuntu/jammy64" nog downloaden.

### Inloggen op de machines

Vagrant doet achter de schermen wat magie om ervoor te zorgen dat bepaalde ssh-keys gemaakt en geinjecteerd worden in de machines.

Je kan erop inloggen met ```vagrant ssh tsvm1```

