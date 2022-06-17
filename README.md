# ci-iaq-workshop

The code samples are tested with Ansible 2.12.6, Terraform 0.15.5, Vagrant 2.2.19, ansible_lint-5.0.12 and Testinfra 6.3.0.

## Setup Local Test Infrastructure
I prepare some Vagrantfiles for the setup of the test infrastructure if necessary.
The only prerequires are that you have to install VirtualBox and Vagrant on your machine.

Then follow these steps:
1. Open a CLI and go to the location of the file Vagrantfile.
2. Call `vagrant up`. Vagrant will download the necessary image for VirtualBox. That will take some times.

Hint: Public and private keys can be generated with the following command: ssh-keygen


## Setup Test Infrastructure in the Cloud with Terraform

If you dont't want to use a local VM. You can also set up a VM in the cloud. 
Here is a sample for Hetzer Cloud. 


```
cd ci-test-vm
terraform init
terraform apply -auto-approve -var="hcloud_token=<your hetzner cloud token>
```

You can clean up your test VM with

```
cd ci-test-vm
terraform destroy  -auto-approve -var="hcloud_token=<your hetzner cloud token>
```

## Ansible Playbooks

The sample playbook uses collection from the community, that is not in the base installation of Ansible.
Therefore, this collection has to be install locally.
Which collection should be installed is defined in `requirements.yml`

```
ansible-galaxy collection install -r requirements.yml
```

After that the Ansible playbook can run with

```
ansible-playbook -i inventory/test.hcloud.yml install-hero-app.yml
```
The flag `-i` defines which server should be provisioned.
In this case, a vm bootstrapped by Terraform in the Hetzner Cloud.

`ansible.cfg` defines default values for Ansible.
In this case, the default remote user is `root` and the default private ssh key is `~/.ssh/id_hetzner_ansible_test`.
If you want to override it, use the flag `-u` for changing the user and the flag `--private-key` for changing the pricate ssh key or change the default value.

## Linting Ansible Playbooks with Ansible Lint

```shell
ansible-lint *.yml
```

## Testing Ansible Playbooks with Testinfra

```
py.test --connection=ansible --ansible-inventory inventory/test.hcloud.yml --hosts='ansible://ansible-test-instance' --force-ansible -v test/*.py
```
