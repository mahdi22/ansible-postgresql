# Ansible role `postgresql`


An Ansible role for installing Postresql on Linux for RHEL/CentOS, Debian, Ubunut and SUSE SLES distributions. Specifically, the responsibilities of this role are to:

- Install Postgresql
- Configure Postgresql parameters and authentifications
- Create users
- Create databases

## Installation
``` bash
$ ansible-galaxy install mahdi22.postgresql
```

## Role Variables
variable/main.yml
Set postgresql configuration parameters or use default values
```Yaml
postgresql_config:
  - option: port
    value: 5432
  - option: log_destination
    value: syslog
  - option: log_directory
    value: /var/log/postgresql/
```
Set or modify postgresql authentification parameters or use default values
```Yaml
postgresql_authentication:
  - {type: local, database: all, user: postgres, auth_method: peer}
  - {type: local, database: all, user: all, auth_method: md5}
  - {type: host, database: all, user: all, address: '127.0.0.1/32', auth_method: md5}
  - {type: host, database: all, user: all, address: '::1/128', auth_method: md5}
  - {type: local, database: replication, user: all, auth_method: peer}
  - {type: host, database: replication, user: all, address: '127.0.0.1/32', auth_method: md5}
  - {type: host, database: replication, user: all, address: '::1/128', auth_method: md5}
```
Set postgresql databases and users to create if not set create_users: no and create_databases: no

Example of creating databases and users:  
```Yaml
postgresql_databases_users:
  - {database: database1, user: user1, userpassword: user1password, priv: ALL}
  - {database: database1, user:'', userpassword: '', priv: ''}
  - {database: '', user: user3, userpassword: user3password, priv: ''}
```
- To create only database without user set database: name, user:''
- To create only user set database: '', user: name, userpassword: password
- To create database with user PRIVILEGES set database: name, user: name, userpassword: password, priv: privileges
### Basic configuration

| Variable                       | Default                       | Comments                                                     |
| :---                           | :---                          | :---                                                         |
| `http_proxy     `              | 'http://proxy.lab.local:8080/'| Set Proxy server and port replace proxy.lab.local:8080      |
| `https_proxy`                  | 'http://proxy.lab.local:8080/'| Set Proxy server and port replace proxy.lab.local:8080      |
| `postgresql_version`           |                               | Postgresql version that will be installed                   |
| `listen_addresses_host_ip`     | yes                           | yes to active postgresql listen on network IP interface     |
| `create_users`                 | yes                           | create postgresql users configured in variable files        |
| `create_databases`             | yes                           | create postgresql databases configured in variable files    |

#### Remarks

(1) Remove the folowing parameters on all install packages tasks if machines have an internet access without proxy server .:
```yaml
environment: "{{ proxy_env }}"
```

## Example Playbook

```Yaml
- hosts: dbservers
  roles:
    - role: mahdi22.postgresql
      become: yes
```

## Testing

This role is tested on these Linux distributions:

- RHEL/CentOS 8   postgresql version  (9.5, 9.4, 10, 11, 12, 13)
- RHEL/CentOS 7   postgresql version  (9.5, 9.4, 10, 11, 12, 13)
- Debian 10       postgresql version  (9.5, 9.4, 10, 11, 12, 13)
- Debian 9        postgresql version  (9.5, 9.4, 10, 11, 12, 13)
- Ubuntu 20.04    postgresql version  (9.5, 9.4, 10, 11, 12, 13)
- Ubuntu 18.04    postgresql version  (9.5, 9.4, 10, 11, 12, 13)
- Ubuntu 16.04    postgresql version  (9.5, 9.4, 10, 11, 12, 13)
- Suse SLES 12    postgresql version  (9.5, 9.4, 10, 11, 12)
- Suse SLES 15    postgresql version  (11, 12, 13)