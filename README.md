Ansible role: unattended\_upgrades
=================================

Installs unattended upgrades for apt and allows the default configuration to be overrided.

Role Variables
--------------

```
ENTRY POINT: main - Install and configure apt unattended upgrades

        Installs unattended upgrades for apt and allows the default
        configuration to be overrided.

OPTIONS (= is mandatory):

- unattended_upgrades_config
        Contents of the unattended upgrades configuration file, or
        empty string to leave file as is
        default: ''
        type: str

- unattended_upgrades_config_template
        Template file for the unattended upgrades configuration
        default: unattended-upgrades
        type: str
```

Installation
------------

This role can either be installed manually with the ansible-galaxy CLI tool:

    ansible-galaxy install git+https://github.com/wandansible/unattended_upgrades,main,wandansible.unattended_upgrades
     
Or, by adding the following to `requirements.yml`:

    - name: wandansible.unattended_upgrades
      src: https://github.com/wandansible/unattended_upgrades

Roles listed in `requirements.yml` can be installed with the following ansible-galaxy command:

    ansible-galaxy install -r requirements.yml

Example Playbook
----------------

    - hosts: all
      roles:
         - role: wandansible.unattended_upgrades
           become: true
           vars:
             unattended_upgrades_config: |
               APT::Periodic::Unattended-Upgrade "1";
               APT::Periodic::Update-Package-Lists "1";
               APT::Periodic::AutocleanInterval "7";
             
               Unattended-Upgrade::Origins-Pattern {
                 "site=apt.grafana.com,a=stable,c=main";
                 "site=download.docker.com,o=Docker,c=stable";
                 "site=dl.cloudsmith.io,o=cloudsmith/caddy/stable,c=main";
               };
             
               Unattended-Upgrade::Remove-Unused-Kernel-Packages "true";
               Unattended-Upgrade::Remove-Unused-Dependencies "true";
             
               Unattended-Upgrade::Mail "sysadmin@example.org";
               Unattended-Upgrade::MailReport "only-on-error";
