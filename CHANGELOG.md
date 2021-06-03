<a name="unreleased"></a>
## [Unreleased]


<a name="1.0.6"></a>
## [1.0.6] - 2021-06-03
### Fixed
- Cleanup task with wrong handler
- Missing zabbix_server_dbschema default variable


<a name="1.0.5"></a>
## [1.0.5] - 2021-05-24
### Added
- zabbix_api_host_dns variable

### Changed
- Improved CI
- Improved zabbix-api package install with run_once
- Use python3 for zabbix_component_dependencies

### Fixed
- sudo right for zabbix_agent_scripts incorrect


<a name="1.0.4"></a>
## [1.0.4] - 2021-04-16
### Changed
- Use python3 for zabbix_component_dependencies

### Fixed
- sudo right for zabbix_agent_scripts incorrect


<a name="1.0.3"></a>
## [1.0.3] - 2021-04-02
### Added
- zabbix_api_host_dns variable


<a name="1.0.2"></a>
## [1.0.2] - 2021-03-29
### Changed
- Updated README.md
- Unified sudo rights in single file
- Improved gitlab-ci

### Fixed
- Regression, zabbix_api tasks were not running following 2b43f85
- Apt pin files not standardized


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


[Unreleased]: https://git.tools01.noxinmortus.fr/sysadmins/ansible/role-zabbix/compare/1.0.6...HEAD
[1.0.6]: https://git.tools01.noxinmortus.fr/sysadmins/ansible/role-zabbix/compare/1.0.5...1.0.6
[1.0.5]: https://git.tools01.noxinmortus.fr/sysadmins/ansible/role-zabbix/compare/1.0.4...1.0.5
[1.0.4]: https://git.tools01.noxinmortus.fr/sysadmins/ansible/role-zabbix/compare/1.0.3...1.0.4
[1.0.3]: https://git.tools01.noxinmortus.fr/sysadmins/ansible/role-zabbix/compare/1.0.2...1.0.3
[1.0.2]: https://git.tools01.noxinmortus.fr/sysadmins/ansible/role-zabbix/compare/1.0.1...1.0.2
