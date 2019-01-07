Role Name
=========

Thruk interface for nagios.

### To easily replace thruk logo and add backgrounds to panorama view

*Enable var:*

```yaml
# In your group_vars/thruk_group/vars.yml
thruk_copy_backgrounds: true
```

Wherever your site.yml (playbook) is, create subfolder: thruk_company_logos, with these files:

    - logo_thruk.png         246x89
    - logo_thruk_small.png   139x50
    - logo_thruk_mid.png     130x40

It will replace thruk logo with yours own.

Same if you want to have copied background for "panorama view" inside thruk, with folder: thruK_backgrounds

     Add any .png file here, it will be available as background for your panoramio.

Requirements
------------

For redhat it requires EPEL repo added already.
For Ubuntu it adds the required repository

Define the variables described below.

    ansible-galaxy install

    ansiblecoffee.nagios_server
    ANXS.mysql

Role Distribution support
------------------------

Ubuntu: ok
RedHat: Not yet

Role Variables
--------------


Variables defined on hosts file
-------------------------------

mysql_root_password: 
thruk_mysql_password:


Dependencies
------------

    ANXS.Mysql
    ansiblecoffee.nagios_server

Example Playbook
----------------

Minimum usage:

    - hosts: servers
      roles:
        - ANXS.mysql
        - nagios_server
        - nagios_server_thruk

Complete list of roles for your site:

``` yaml
- name: apply Nagios settings
  hosts: nagios_servers
  become: yes
  become_method: sudo
  roles:
    - { role: nagios_server, tags: ["install", "nagios_server_all", "nagios_server"] }
    - { role: nagios_server_plugins, tags: ["install", "nagios_server_all", "nagios_server_plugins"] }
    - { role: nagios_server_pnp4nagios, tags: ["install", "nagios_server_all", "nagios_server_pnp4nagios"] }
    - { role: ANXS.mysql, tags: ["install", "nagios_server_all", "nagios_server_thruk", "ANXS.mysql"] }
    - { role: nagios_server_thruk, tags: ["install", "nagios_server_all", "nagios_server_thruk"] }
    - { role: postfix_client, tags: ["install", "nagios_server_all", "postfix_client"] }
# Additional tags: role/tag
# nagios_server             - config_nagios
# nagios_server             - nagios_server_main_config
# nagios_server             - config_nagios_cron
# nagios_server_plugins     - config_nagios_plugins
# nagios_server_plugins     - test_nagios_plugins
# nagios_server_pnp4nagios  - test_nagios_pnp4nagios
# nagios_server_thruk       - config_nagios_thruk_cron
# nagios_server_thruk       - test_nagios_thruk
# nagios_server_thruk_git   - config_nagios_thruk_git_cron
```

### Tags

We prefer to tag roles instead of tasks (see above), but sometimes few tasks are useful to have a tag when you implement a small change.

    config_nagios_thruk_cron
    test_nagios_thruk

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).


Migrate data from previos nagios
--------------------------------

    tar -cf backupnagioshistory.tar /var/log/nagios3  # backup nagios3 logs
    tar -xf backupnagioshistory.tar -C /              # Restore nagios3 logs

Import data to thruk
====================

    thruk -a logcacheimport --local /var/log/nagios3/archives/*
    thruk -a logcacheupdate --local /var/log/nagios3/nagios.log

Check more info at: http://www.thruk.org/documentation/logfile-cache.html

Migrate data for pnp4nagios graph from previous server
======================================================

    tar -cf pnp4nagiosbackupfile.tar /usr/local/pnp4nagios/var   # Backup the graphs
    tar -xf pnp4nagiosbackupfile.tar /                           # Restore the graphs

