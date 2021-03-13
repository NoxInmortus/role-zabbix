<a name="unreleased"></a>
## [Unreleased]


<a name="1.0.1"></a>
## 1.0.1 - 2021-03-13
### Added
- Improved CI tasks and fixed several unseen issues
- `bulk` sub-parameter for zabbix_host api task
- StartAgents parameter for zabbix agent
- ServerActive variable for agent & proxy
- EnableRemoteCommands and LogRemoteCommands parameters & Manage zabbix proxies through api
- Install zabbix-sender package

### Changed
- Manage changelog with git-chglog
- Removed always tags and fixed missing tags
- [Breaking Changes] Splitted `zabbix_api_host_groups` and `zabbix_api_host_templates` from `zabbix_api_host` variable for simpler usage. Also changed `zabbix_api_host.inventory_mode` default to `automatic`
- Improved and cleaned tasks/external.yml
- Improved agent/config.yml task & added zabbix_agent_sudo_extra variable

### Fixed
- Regression, removed ServerActive parameter from zabbix proxy


[Unreleased]: https://git.tools01.noxinmortus.fr/sysadmins/ansible/role-zabbix/compare/1.0.1...HEAD
