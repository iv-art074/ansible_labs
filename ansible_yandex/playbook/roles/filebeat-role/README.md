Kibana role
=========

Роль для установки kibana на хостах с ОС: Debian, Ubuntu, CentOS, RHEL.

Requirements
------------

Поддерживаются только ОС семейств debian и EL.

Role Variables
--------------

Variable name	Default	Description
kibana_version	"7.14.0"	Параметр, который определяет какой версии kibana будет установлена

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

```
# requirements.yml
---
- src: git@github.com:maxship/kibana-role.git
  scm: git
  version: "2.0.0"
  name: kibana-role

# site.yml
---
- name: Install Kibana 
  hosts: all
  roles:
    - kibana-role
  tags: kibana
  ``` 
  
License
-------

MIT

