# Zabbix ![Zabbix](https://img.shields.io/badge/Ansible-Zabbix.svg)

## Sommaire
* [Preview](#preview)
  - [TODO](#todo)
  - [Features](#features)
  - [Requirements](#requirements)
  - [Compatibility](#compatibility)
* [Usage](#usage)
  - [Variables](#variables)
  - [Tags](#tags)
  - [EXTERNAL Variables examples](#external-variables-examples)
* [Licence](#licence)
* [Credits](#credits)

## Preview
Ansible role to install Zabbix components on Debian (with MySQL support for proxies/servers).

I was tired of using multiple roles to install and manage zabbix components so I decided to make my own.

Computers, or I could say IT, exist to save working time. Ansible exist to save even more time for System Administrators. So I tried to write an ansible role which could save even more time when working with Zabbix.

Furthermore, my goal is to make the cleanest, fastest, easily-readable & usable role as possible.

Almost everything this role does is customisable through vars, but you should be able to use it with a very small amount of vars (compared to the available ones).

### TODO
- CI tests
- SNMP
- Test zabbix_scripts_git with multiple repositories
- Manage host with multiple supervised network interfaces

### Features
- Install Zabbix agent/proxy/server
- Configure Zabbix agent and proxy (not all parameters available but main stuff is there)
- Configure Zabbix server (all parameters available through dict)
- Manage proxies, hosts, groups, maintenances and screens through API
- Deploy scripts and includable configurations through templates or git repositories
- Optional regular update (`true` by default) of the cloned git repositories (scripts/includes) through cron

### Requirements
- Ansible >= 2.5
- MySQL server (for proxy or server)
- MySQL database and user with correct rights (for proxy or server)

### Compatibility
  ![Debian](https://img.shields.io/badge/Debian-Jessie|Stretch|Buster-blue.svg)
  ![Debian](https://img.shields.io/badge/Raspbian-Jessie|Stretch|Buster-blue.svg)

| OS              |ZBX 4.0|ZBX 4.2|ZBX 4.4|
|-----------------|-------|------|--------|
|Debian Jessie    |   X   |  X   |    X   |
|Debian Stretch   |   X   |  X   |    X   |
|Debian Buster    |   X   |  X   |    X   |
|Raspbian Jessie  |   X   |  X   |    X   |
|Raspbian Stretch |   X   |  X   |    X   |
|Raspbian Buster  |   X   |  X   |    X   |

## Usage
### Variables

Note: `zabbix_component` variable does not need to be defined, automatically set as `agent`,`proxy`, or `server`.

|GLOBAL VARIABLES|TYPE|REQUIRED|DEFAULT|DESCRIPTION|
|-|-|-|-|-|
|zabbix_agent|BOOL|NO|FALSE|Install zabbix agent|
|zabbix_proxy|BOOL|NO|FALSE|Install zabbix proxy|
|zabbix_server|BOOL|NO|FALSE|Install zabbix server|
|zabbix_version|STRING|NO|'4.0'| Zabbix version|
|zabbix_servers|LIST|YES|NONE| IP address (or hostnames) of Zabbix proxies/servers|
|zabbix_repository|STRING|NO|See defaults|Zabbix repository|
|zabbix_apt_key|STRING|NO|'https://repo.zabbix.com/zabbix-official-repo.key'|Zabbix repository apt key|
|zabbix_hostname|STRING|NO|inventory_hostname|Unique, case sensitive host name|
|zabbix_conf_dir|STRING|NO|/etc/zabbix|Main zabbix directory|
|zabbix_conf_file|STRING|NO|`/etc/zabbix/zabbix_{{ zabbix_component }}.conf`|Zabbix config file|
|zabbix_conf_include_dir|STRING|NO|`/etc/zabbix/zabbix_{{ zabbix_component }}d.conf.d`|Zabbix include directory|
|zabbix_conf_include|STRING|NO|`{{ zabbix_conf_include_dir }}/*.conf`|Zabbix include files|
|zabbix_listenport|INT|NO|10051|Zabbix port for proxy/server|
|zabbix_log_dir|STRING|NO|/var/log/zabbix|Zabbix log directory|
|zabbix_logfile|STRING|NO|`{{ zabbix_log_dir }}/zabbix_{{ zabbix_component }}.log`|Zabbix log file|
|zabbix_logfilesize|INT|NO|0|Zabbix log file size limit (0 is feature disabled)|
|zabbix_pidfile|STRING|NO|`/run/zabbix/zabbix_{{ zabbix_component }}.pid`|Zabbix pid file|
|zabbix_debuglevel|INT|NO|1|Zabbix debug log level|
|zabbix_timeout|INT|NO|30|Zabbix timeout|
|zabbix_scripts|STRING|NO|/etc/zabbix/zabbix-scripts|Zabbix scripts path|
|zabbix_dbsocket|STRING|NO|/var/run/mysqld/mysqld.sock|Zabbix db socket for mysql connections|
|zabbix_dbport|INT|NO|3306|Zabbix db port for mysql connections|
|zabbix_dbhost|STRING|NO|'127.0.0.1'|Zabbix dbhost for proxies/server|
|zabbix_dbname|STRING|NO|`zabbix_{{ zabbix_component }}`|Zabbix dbname for proxies/server|
|zabbix_dbuser|STRING|NO|`zabbix_{{ zabbix_component }}`|Zabbix dbuser user for proxies/server|
|zabbix_user|STRING|NO|zabbix|Zabbix user|
|zabbix_tls_dir|STRING|NO|/etc/zabbix/tls|Zabbix tls directory|
|zabbix_tls_pskfile|STRING|NO|/etc/zabbix/tls/psk.secret|Zabbix psk file|
|zabbix_configfrequency|INT|NO|3600|Zabbix global config frequency|
|zabbix_unreacheableperiod|INT|NO|300|Zabbix global unreacheable period|
|zabbix_dependencies|LIST|NO|See defaults|Zabbix dependencies packages to install|

|AGENT VARIABLES|TYPE|REQUIRED|DEFAULT|DESCRIPTION|
|-|-|-|-|-|
|zabbix_agent_servers|LIST|PERHAPS|`{{ zabbix_servers }}`|Zabbix agent overwrite variable for zabbix_servers|
|zabbix_agent_servers_ative|LIST|PERHAPS|`{{ zabbix_servers }}`|Zabbix agent overwrite variable for zabbix_servers|
|zabbix_agent_listenport|INT|NO|10050|Zabbix agent listen port|
|zabbix_agent_hostname|STRING|NO|`{{ zabbix_hostname }}`|Zabbix agent hostname overwrite variable|
|zabbix_agent_timeout|INT|NO|`{{ zabbix_timeout }}`|Zabbix agent timeout overwrite variable|
|zabbix_agent_debuglevel|INT|NO|`{{ zabbix_debuglevel }}`|Zabbix agent debug log level overwrite variable|
|zabbix_agent_pidfile|STRING|NO|`/run/zabbix/zabbix_{{ zabbix_component }}d.pid`|Zabbix pid file overwrite variable|
|zabbix_agent_scripts|STRING|NO|`{{ zabbix_scripts }}`|Zabbix agent scripts overwrite variable|
|zabbix_agent_sudo|BOOL|NO|true|Add all sudoers right to exec scripts in `{{ zabbix_agent_scripts }}`|
|zabbix_agent_tlsconnect|STRING|NO|unencrypted|Zabbix agent tls connection mode|
|zabbix_agent_tlsaccept|STRING|NO|unencrypted|Zabbix agent tls accept mode|
|zabbix_agent_pskid|STRING|NO|None|Zabbix agent psk id|
|zabbix_agent_psksecret|STRING|NO|None|Zabbix psk secret. Use `openssl rand -hex 64` to generate a secret|
|zabbix_agent_remotecommands|INT|`0`|Enable remote commands|
|zabbix_agent_logremotecommands|INT|`0`|Log remote commands|
|zabbix_agent_startagents|INT|`8`|StartAgents parameter|

|PROXY VARIABLES|TYPE|REQUIRED|DEFAULT|DESCRIPTION|
|-|-|-|-|-|
|zabbix_proxy_servers|LIST|PERHAPS|`{{ zabbix_servers }}`|Zabbix proxy overwrite variable for zabbix_servers|
|zabbix_proxy_servers_ative|LIST|PERHAPS|`{{ zabbix_servers }}`|Zabbix proxy overwrite variable for zabbix_servers|
|zabbix_proxy_mysql_import|BOOL|NO|false|Import proxy mysql schema|
|zabbix_proxy_mode|INT|NO|0|Zabbix proxy mode 0=active / 1=passive|
|zabbix_proxy_listenport|INT|NO|`{{ zabbix_listenport }}`|Zabbix proxy overwrite variable for listening port|
|zabbix_proxy_serverport|INT|NO|`{{ zabbix_listenport }}`|Zabbix proxy overwrite variable for server port|
|zabbix_proxy_timeout|INT|NO|`{{ zabbix_timeout }}`|Zabbix proxy timeout overwrite variable|
|zabbix_proxy_debuglevel|INT|NO|`{{ zabbix_debuglevel }}`|Zabbix proxy debug log level overwrite variable|
|zabbix_proxy_scripts|STRING|NO|`{{ zabbix_scripts }}`|Zabbix proxy scripts overwrite variable|
|zabbix_proxy_configfrequency|INT|NO|`{{ zabbix_configfrequency }}`|Zabbix proxy config frequency overwrite variable|
|zabbix_proxy_unreacheableperiod|INT|NO|`{{ zabbix_unreacheableperiod }}`|Zabbix proxy unreacheable period overwrite variable|
|zabbix_proxy_dbschema|STRING|NO|/usr/share/doc/zabbix-proxy-mysql/schema.sql.gz|Zabbix proxy db schema path|
|zabbix_proxy_tlsconnect|STRING|NO|unencrypted|Zabbix proxy tls connection mode|
|zabbix_proxy_tlsaccept|STRING|NO|unencrypted|Zabbix proxy tls accept mode|
|zabbix_proxy_pskid|STRING|NO|None|Zabbix proxy psk id|
|zabbix_proxy_psksecret|STRING|NO|None|Zabbix psk secret. Use `openssl rand -hex 64` to generate a secret|
|zabbix_proxy_dbsocket|STRING|NO|`{{ zabbix_dbsocket }}`|Zabbix db socket for mysql connection overwrite variable|
|zabbix_proxy_dbport|INT|NO|`{{ zabbix_dbport }}`|Zabbix db port for mysql connection overwrite variable|
|zabbix_proxy_dbhost|INT|NO|`{{ zabbix_dbhost }}`|Database hostname for mysql schema import overwrite variable|
|zabbix_proxy_dbname|STRING|NO|`{{ zabbix_dbname }}`|Database name for mysql schema import overwrite variable|
|zabbix_proxy_dbuser|STRING|NO|`{{ zabbix_dbuser }}`|Database user for mysql schema import overwrite variable|
|zabbix_proxy_dbpassword|STRING|PERHAPS|None|Database password for mysql schema import|
|zabbix_proxy_remotecommands|INT|`0`|Enable remote commands|
|zabbix_proxy_logremotecommands|INT|`0`|Log remote commands|

|SERVER VARIABLES|TYPE|REQUIRED|DEFAULT|DESCRIPTION|
|-|-|-|-|-|
|zabbix_server_mysql_import|BOOL|NO|false|Import server mysql schema|
|zabbix_server_conf_default|DICT|NO|See defaults|Defaults zabbix server variables|
|zabbix_server_conf|DICT|PERHAPS|None|Additional zabbix server variables|
|zabbix_server_dbpassword|STRING|YES|None|MySQL DBPassword for zabbix server|
|zabbix_server_psksecret|STRING|NO|None|Zabbix psk secret. Use `openssl rand -hex 64` to generate a secret|

|EXTERNAL VARIABLES|TYPE|REQUIRED|DEFAULT|DESCRIPTION|
|-|-|-|-|-|
|zabbix_agent_include_templates|STRING|NO|None|Do it my way, use fileglob to deploy templates of includes configurations files for agent|
|zabbix_proxy_include_templates|STRING|NO|None|Do it my way, use fileglob to deploy templates of includes configurations files for proxy|
|zabbix_server_include_templates|STRING|NO|None|Do it my way, use fileglob to deploy templates of includes configurations files for server|
|zabbix_include_git|DICT|NO|None|Get Zabbix includes configurations from git repositories|
|zabbix_scripts_templates|LIST|NO|None|Deploy Zabbix scripts from templates|
|zabbix_scripts_git|DICT|NO|None|Get zabbix scripts from git repostories|
|zabbix_external_cron|DICT|NO|See defaults|DICT for crons configuration|
|zabbix_external_git_cron|BOOL|NO|`true`|Regular update of scripts/templates cloned through git|

|API VARIABLES|TYPE|REQUIRED|DEFAULT|DESCRIPTION|
|-|-|-|-|-|
|zabbix_api|DICT|YES|See defaults/main.yml|Dict for zabbix api connection|
|zabbix_api.zabbix_url|STRING|YES|NONE|Zabbix URL for zabbix api connection|
|zabbix_api.zabbix_api_user|STRING|YES|NONE|Zabbix user for zabbix api connection|
|zabbix_api.zabbix_api_password|STRING|YES|NONE|Zabbix password for zabbix api connection|
|zabbix_api_host_ip|STRING|NO|`"{{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}"`|Zabbix host listening IP address|
|zabbix_api_maintenance|DICT|NO|NONE|Dict to defined maintenance windows|
|zabbix_api_host_interfaces|DICT|NO|See defaults/main.yml|Dict for zabbix host interfaces parameters|
|zabbix_api_host|DICT|YES|See defaults/main.yml|Dict for zabbix host parameters, see https://docs.ansible.com/ansible/latest/modules/zabbix_host_module.html for configuration|
|zabbix_api_host.proxy_status|STRING|NO|NONE|Zabbix proxy mode, can be `active` or `passive`|
|zabbix_api_host.proxy_state|STRING|NO|NONE|Zabbi proxy state, can be `present` or `absent`|

### EXTERNAL Variables examples

Using `zabbix_include_templates` you can deploy templates configurations files from your ansible controller with something like :
```
zabbix_include_templates: "../data/zabbix/*.j2"
```

Using `zabbix_include_git` you can deploy includes configurations files from git repositories.
```
zabbix_include_git:
  - repo: 'https://foosball.example.org/path/to/repo.git'
    version: dev
  - repo: 'https://gitlab.com/myname/myrepo.git'
    version: prod
    force: yes

zabbix_scripts_templates:
  - "../data/scripts/myscript.sh"
  - "../data/zbx/scripts/pyfoo.py"

zabbix_scripts_git:
  - repo: 'https://foosball.example.org/path/to/repo.git'
    version: dev
  - repo: 'https://gitlab.com/myname/myrepo.git'
    version: prod
    force: yes
```

### API Variables examples
```
zabbix_api:
  url: 'https://zabbix.mydomain.com'
  username: 'user'
  password: 'P@55w0rd'

# Specify ip pub if behind a router
zabbix_api_host_ip: 163.172.145.120

zabbix_api_host:
  groups:
    - MyNewGroup
  templates:
    - OS_LINUX
    - MY_CUSTOM_TEMPLATE
  tls_accept: 2
  tls_connect: 2
```

### Tags

You can use  `--tags` and `--skip-tags`

|TAG|DESCRIPTION|
|-|-|
|zabbix_agent|Agent tasks|
|zabbix_proxy|Proxy tasks|
|zabbix_server|Server tasks|
|zabbix_install|Installation tasks|
|zabbix_import| Initial import database schema|
|zabbix_config|Configurations tasks|
|zabbix_external|External tasks (includes & scripts deploy)|
|zabbix_cleanup|Cleanup zabbix directory of unneeded files and directories|
|zabbix_api|API tasks|
|zabbix_api_hosts|API hosts task|
|zabbix_api_maintenance|API maintenances task|
|zabbix_api_screen|API screens task|
|zabbix_api_proxies|API proxies task|

## Sources
- https://www.zabbix.com/documentation/current/manual/appendix/config/zabbix_agent
- https://www.zabbix.com/documentation/current/manual/appendix/config/zabbix_proxy
- https://www.zabbix.com/documentation/current/manual/appendix/config/zabbix_server
- https://docs.ansible.com/ansible/latest/modules/zabbix_host_module.html
- https://docs.ansible.com/ansible/latest/modules/zabbix_group_module.html
- https://docs.ansible.com/ansible/latest/modules/zabbix_proxy_module.html
- https://docs.ansible.com/ansible/latest/modules/zabbix_screen_module.html
- https://docs.ansible.com/ansible/latest/modules/zabbix_maintenance_module.html
- Changelog made with : https://github.com/rustic-games/jilu

## Licence
MIT view [LICENSE](LICENSE)

## Credits
Alban E.G.
