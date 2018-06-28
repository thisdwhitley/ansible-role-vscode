Ansible Role: vscode
====================

[![Build Status](https://travis-ci.org/dswhitley/ansible-role-vscode.svg?branch=master)](https://travis-ci.com/dswhitley/ansible-role-vscode)

This role will install and potentially configure the *open source* [Visual
Studio Code](https://code.visualstudio.com/) editor.  Currently supported
distributions (per
[requirements](https://code.visualstudio.com/docs/supporting/requirements)):

* Red Hat/CentOS 7+
* Fedora 24+
* Debian 9+
* Ubuntu 16.04+

I prefer to pass the variables "into" the role from the playbook versus by
including variable files.  This is because I hope to make the role usable by
other projects/roles.  I don't know if this logic makes sense or not, but I am
essentially attempting to remove the variables from the role itself.

This will be installed via a repository and therefore updated similarly so the
exact version does not need to be specified.  Previously I was unaware of this
but I will be adhering to the guidelines here:
[Running VS Code on Linux](https://code.visualstudio.com/docs/setup/linux)

Important Notes
---------------

* This role started totally differently.  But in using `code` regularly, I have
  found that updates come often and that it is not worth updating the role for
  each new version.  So this role will install whatever version is in the
  repository (knowing that it might not be the absolute latest).
* This is also my first foray into using Travis for CI

Requirements
------------

Any package requirements (generally for extensions) will be addressed in the
role.

Role Variables
--------------

All of these variables should be considered **optional** however, be aware that
sanity checking is minimal (if at all):

* `users` *ideally you could configure Visual Studio Code differently per user,
  and this nested list of users allows for that*
  * `username`
    * this is the username **NOTE: this role will not create the user!**
  * `vscode_extensions`
    * this is a ***list*** of extensions to install *see example playbook*
  * `vscode_settings`
    * this is a ***dictionary*** of settings applied via a template

Example Playbook
----------------

Playbook with configuration options specified:

```yaml
- hosts: localhost
  connection: local
  roles:
    - role: vscode
      users:
        - username: test_user1
          vscode_extensions:
            - vscoss.vscode-ansible
            - stayfool.vscode-asciidoc
            - dbankier.vscode-instant-markdown
            - wholroyd.jinja
            - DavidAnson.vscode-markdownlint
            - ms-python.python
            - AndrsDC.base16-themes
          vscode_settings: {
            "editor.rulers": [80, 100, 120],
            "editor.renderWhitespace": true,
            "files.associations": {
              "Vagrantfile": "ruby"
            }
          }
```

Inclusion
---------

I envision this role being included in a larger project through the use of a
`requirements.yml` file.  So here is an example of what you would need in your
file:

```yaml
# get the builder role from gitlab
- src: https://github.com/dswhitley/ansible-role-vscode.git
  scm: git
  name: vscode
```

Have the above in a `requirements.yml` file for your project would then allow
you to "install" it (prior to use in some sort of setup script?) with:

```bash
ansible-galaxy install -p ./roles -r requirements.yml
```

Testing
-------

I am relying heavily on the work by Jeff Geerling in using Travis CI for testing
this role.  I have, however, modified both `tests/test.yml` and `.travis.yml` in
order to include testing of the installation of extensions as well as config
changes.  Please review those files, but here is a list of OSes currently being
tested (using geerlingguy's container images):

* centos7
* fedora27
* ubuntu1804
* ubuntu1604
* debian9

Reference:  [How I test Ansible configuration on 7 different OSes with Docker](https://www.jeffgeerling.com/blog/2018/how-i-test-ansible-configuration-on-7-different-oses-docker)

To-do
-----

N/A

References
----------

* [GantSign's role](https://github.com/gantsign/ansible-role-visual-studio-code)

License
-------

All parts of this project are made available under the terms of the [MIT
License](LICENSE).