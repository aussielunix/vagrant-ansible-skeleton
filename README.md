## Ansible Project

This repo holds the ansible playbooks/roles to manage....

## Project Setup

The tools used in this project will require the following before you start:

* python2

Setup an python virtualenv (optional)

```
virtualenv . --distribute
source bin/activate
```

Install any python2 dependencies (ansible etc).

```
pip install -r requirements.txt
```

## Making Ansible Changes

Anisble is pretty straight forward as far as configuration management tools.

* Edit some yaml files or jinja2 templates.
* test in vagrant
* push out to host(s)

Directories/Files that matter:

* inventories/
  In here is an file/files that lets Ansible know what hosts we are managing and what group name we have them listed under.
  ```
  [master]
  foo.bar.au
  ```
* playbooks/
  High level instructions that glue together a bunch of 'roles'

* roles/
  Groups of ansible tasks/plays/handlers. Each role should be componentised and focus on one topic only.

## Further reading

http://docs.ansible.com/ is an excellent collection of documentation/reference material.

## Testing changes locally

There is an included vagrant environment for testing these changes locally.  
Vagrant reads the virtualbox settings from [settings.yml](https://github.com/aussielunix/vagrant-ansible-skeleton/blob/master/settings.yml)

## Rolling out to servers


``` bash
ansible-playbook -i inventories/hosts playbooks/moo.yml

```

## Contributing

See [CONTRIBUTING](CONTRIBUTING.md) for contributing to this repo.

