Ansible Role: virtualenvwrapper 
======================================

[![Build Status](https://travis-ci.org/entercloudsuite/ansible-virtualenvwrapper.svg?branch=master)](https://travis-ci.org/entercloudsuite/ansible-virtualenvwrapper)
[![Galaxy](https://img.shields.io/badge/galaxy-entercloudsuite.virtualenvwrapper-blue.svg?style=flat-square)](https://galaxy.ansible.com/entercloudsuite/virtualenvwrapper)  

Installs virtualenvwrapper on Ubuntu 16.04 (Xenial)

## Requirements

This role requires Ansible 2.4 or higher.

## Role Variables

The role defines most of its variables in `defaults/main.yml`:

### venv_venv_home:
default: /usr/venv
### virtualenvwrapper_scrpit_path
default: /usr/local/bin/virtualenvwrapper.sh
### bashrc_path
default: /root/.bashrc

## Example Playbook

Run with default vars:
```yaml
    - hosts: all
      roles:
        - { role: ansible-virtualenvwrapper }
```

## What use for prepare the virtualENV
##### Example where install virtualenv and prepare a virtualenv for website1

```yaml
---
- name: run the main role
  hosts: all
  become_method: su
  roles:
    - {role: ansible-virtualenvwrapper}
    - role: cchurch.virtualenv
      virtualenv_path: /usr/venv/website1
      virtualenv_os_packages:
        apt: [libjpeg-dev]
        yum: [libjpeg-devel]
      virtualenv_pre_packages:
        - name: Django
          version: 1.8.18
        - Pillow
      virtualenv_requirements:
        - /var/www/mywebsite_path/requirements.txt
      virtualenv_post_packages:
        - name: PIL
          state: absent

```


## Testing

Tests are performed using [Molecule](http://molecule.readthedocs.org/en/latest/).

Install Molecule or use `docker-compose run --rm molecule` to run a local Docker container, based on the [enterclousuite/molecule](https://hub.docker.com/r/fminzoni/molecule/) project, from where you can use `molecule`.

1. Run `molecule create` to start the target Docker container on your local engine.  
2. Use `molecule login` to log in to the running container.  
3. Edit the role files.  
4. Add other required roles (external) in the molecule/default/requirements.yml file.  
5. Edit the molecule/default/playbook.yml.  
6. Define infra tests under the molecule/default/tests folder using the goos verifier.  
7. When ready, use `molecule converge` to run the Ansible Playbook and `molecule verify` to execute the test suite.  
Note that the converge process starts performing a syntax check of the role.  
Destroy the Docker container with the command `molecule destroy`.   

To run all the steps with just one command, run `molecule test`. 

In order to run the role targeting a VM, use the playbook_deploy.yml file for example with the following command: `ansible-playbook ansible-virtualenvwrapper/molecule/default/playbook_deploy.yml -i VM_IP_OR_FQDN, -u ubuntu --private-key private.pem`.  

## License

MIT
