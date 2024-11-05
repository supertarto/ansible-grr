# Ansible GRR
[![CI](https://github.com/supertarto/ansible-grr/actions/workflows/ci.yml/badge.svg)](https://github.com/supertarto/ansible-grr/actions/workflows/ci.yml)

Install and configure GRR with ansible. Tested with GRR *>=v4.1.0*

## Requirement
A webserver, php >= 7.4 and a database are required. You can use supertarto.apache, supertarto.php and supertarto.mariadb. But other roles are just fine.
For PHP, those modules are needed:
* common
* mysql
* gd
* mbstring
* curl
* json
* ldap
* pdo
* xml

You can have more information on the application official repository:
https://github.com/JeromeDevome/GRR

## Tested plateform
* Debian 12 (Bookworm)

## Role variables

Informations needed for the installation. Force_update will create a backup_dir and overwite your previous grr directories.
```yml
grr_download_url: "https://github.com/JeromeDevome/GRR/releases/tag/{{ grr_version }}"
grr_unarchive_dir: "/var/www"
grr_content_dest: "{{ grr_unarchive_dir }}/grr"
grr_version: "v4.3.7"
grr_force_update: false
grr_backup_directory: "/var/www/grr-bck"
```

The web user and group
```yml
grr_web_owner: www-data
grr_web_group: www-data
```

Your database informations.
```yml
grr_db_host: "localhost"
grr_db_name: "grrbase"
grr_db_user_name: "grruser"
grr_db_password: "grrpassword"
grr_table_prefix: ""
grr_db_port: "3306"
```

If you want to use LDAP for authentification, set grr_use_ldap to true.
The LDAP protocol can be either ldap or ldaps
If you want to use TLS for your LDAP connection, set grr_ldap_tls to "TRUE". But, beware, it must me an sting and not a boolean.
```yml
grr_use_ldap: false
grr_ldap_protocol: "ldap"
grr_ldap_adress: localhost
grr_ldap_port: "389"
grr_ldap_login: ""
grr_ldap_password: ""
grr_ldap_base: ""
grr_ldap_filter: ""
grr_ldap_tls: "FALSE"
```
This value represent the range before and after the actual year. For example, if you set "10" and if the actual year is 2020, you will see calendar from 2010 to 2030.
```yml
grr_config_year_calendar: "10"
```
Set your default timezone
```yml
grr_config_default_timezone: "Europe/Paris"
```
If your ntp server use winter time, set to 1. Else, 0.
```yml
grr_config_correct_winter_time: "1"
```
Assign a default domain base on your client IP adress. Set 1 if you want to use this feature.
```yml
grr_config_default_domain_by_ip: "0"
```
Maximum number (+1) of periodic reservation allowed.
```yml
grr_config_max_periodic_reservation: "365 + 1"
```
The administrator can restore a database from the web interface. Set to 0 to forbid this feature.
```yml
grr_config_restore_db_admin_page: "1"
```
Only set to 1 for debug need.
```yml
grr_config_debug_flag: "0"
```
Set to 1 to allow automatic search of update.
```yml
grr_config_search_update: "1"
```
Set to 1 to allow the possibility to force update your installation
```yml
grr_allow_force_update: "0"
```
Number of day the logs are kept.
```yml
grr_config_max_log_keep: "365"
```
Number of day the mails logs are kept.
```yml
grr_config_max_email_day_keep: "365"
```
Set a password if you want to allow the authentification via config.inc.php. Empty if you don't want to use this feature.
```yml
grr_config_auth_with_configinc: ""
```
Hide (or not) the sso configuration interface.
```yml
grr_config_sso_hide_interface: "false"
```
Hide (or not) the ldap configuration interface.
```yml
grr_config_ldap_hide_interface: "false"
```
Hide (or not) the imap authentification configuration interface.
```yml
grr_config_imap_hide_interface: "false"
```
Set the fixed URL that will be set as the CAS service parameter. When this method is not called, a phpCAS script uses its own URL.
```yml
grr_config_CAS_setFixedServiceURL: ""
```

## Installation
```
ansible-galaxy role install supertarto.grr
```

## License
GPL V3.0
