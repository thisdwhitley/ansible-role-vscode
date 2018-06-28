Ansible Role: vscode
====================

This role will install and potentially configure the *open source* [Visual
Studio Code](https://code.visualstudio.com/) editor.  Currently supported
distributions:

* Red Hat/CentOS/Fedora
* Debian/Ubuntu

I prefer to pass the variables "into" the role from the playbook versus by
including variable files.  This is because I hope to make the role usable by
other projects/roles.  I don't know if this logic makes sense or not, but I am
essentially attempting to remove the variables from the role itself.

There is not currently a repository for the package installation. Instead of
making this role version-specific, it will always install the latest version
found here:
[https://code.visualstudio.com/Download](https://code.visualstudio.com/Download)

Important Notes
---------------

* This role started totally differently.  But in using `code` regularly, I have
  found that updates come often and that it is not worth updating the role for
  each new version, but rather simply accept a specific version to install *OR*
  just install the latest version.  
* This is also my first foray into using Travis for CI

Requirements
------------

Any package requirements (generally for extensions) will be addressed in the
role.

Role Variables
--------------

All of these variables should be considered **optional** however, be aware that
sanity checking is minimal (if at all):

* `vscode_version`
  * this allows you to specify a version, but if it is not latest, you will be
    reminded incessantly that there is a newer version avaible, so I usually
    don't specify a version
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
        - username: vagrant
          visual_studio_code_extensions:
            - streetsidesoftware.code-spell-checker
            - wholroyd.jinja
            - donjayamanne.python
          visual_studio_code_settings: {
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