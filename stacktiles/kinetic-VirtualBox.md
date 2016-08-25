# kinetic-VirtualBox Playbook Validation

We are using a base image from Ubuntu Xenial.

# First we should update our apt cache for debian packages

- {{ exec kinetic-VirtualBox: apt-get update }}

# Second we should setup ansible.
We are doing this as root, following [Ansible Documentation](http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu). We also install git at the same time for convenience :

- {{ exec kinetic-VirtualBox: apt-get install software-properties-common -y }}
- {{ exec kinetic-VirtualBox: apt-add-repository ppa:ansible/ansible -y }}
- {{ exec kinetic-VirtualBox: apt-get update }}
- {{ exec kinetic-VirtualBox: apt-get install ansible git -y }}

# (TMP) Third we install all our dependencies for our playbook

Note we cannot currently simply use ansible-pull because of [galaxy dependency issue](https://github.com/ansible/ansible/issues/10636). so we need to clone the git reposotiry and run ansible-galaxy.

- {{ exec kinetic-VirtualBox: git clone https://github.com/asmodehn/playbooks.git playbooks }}
- {{ exec kinetic-VirtualBox: cd playbooks && ansible-galaxy install -r requirements.yml }}


# Then we can run our playbook

{{ exec kinetic-VirtualBox: ansible-pull -U https://github.com/asmodehn/playbooks.git -i \`hostname -s\`, -c local }} 

