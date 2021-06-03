# ci-iaq-workshop

The code samples samples/ansible are tested with Ansible 2.10.9 and Vagrant 2.2.14.

## Setup Local Test Infrastructure
I prepare some Vagrantfiles for the setup of the test infrastructure if necessary.
The only prerequires are that you have to install VirtualBox and Vagrant on your machine.

Then follow these steps:
1. Open a CLI and go to the location of the file Vagrantfile.
2. Call `vagrant up`. Vagrant will download the necessary image for VirtualBox. That will take some times.

Hint: Public and private keys can be generated with the following command: ssh-keygen


## Ansible Playbooks

The sample playbook uses collection from the community, that is not in the base installation of Ansible.
Therefore, this collection has to be install locally.
Which collection should be installed is defined in `requirements.yml`

```
ansible-galaxy collection install -r requirements.yml
```

After that the Ansible playbook can run with

```
ansible-playbook -i inventory/vagrant install-hero-app.yml
```
The flag `-i` defines which server should be provisioned.
In this case, the local vm bootstrapped by Vagrant.

`ansible.cfg` defines default values for Ansible.
In this case, the default remote user is `vagrant`.
If you want to override it, use the flag `-u` for that or change the default value.
