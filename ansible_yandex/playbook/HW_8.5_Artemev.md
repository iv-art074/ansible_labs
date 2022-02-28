##### Инициализация default scenario 
```
iv_art@Pappa:/mnt/c/Users/iv_ar/Documents/GitHub/ansible_labs/ansible_yandex/playbook/roles/elasticsearch-role$ molecule init scenario --role-name elasticsearch-role --driver-name docker
INFO     Initializing new scenario default...
INFO     Initialized scenario in /mnt/c/Users/iv_ar/Documents/GitHub/ansible_labs/ansible_yandex/playbook/roles/elasticsearch-role/molecule/default successfully.
```
При запуске molecule test получаем  
```
TASK [elasticsearch-role : include_tasks] **************************************
fatal: [instance]: FAILED! => {"reason": "Could not find or access '/mnt/c/Users/iv_ar/Documents/GitHub/ansible_labs/ansible_yandex/playbook/roles/elasticsearch-role/molecule/default/download_dnf.yml' on the Ansible Controller."}

PLAY RECAP *********************************************************************
instance                   : ok=1    changed=0    unreachable=0    failed=1    skipped=1    rescued=0    ignored=0
```
что говорит об отсутствии менеджера пакетов dnf  

2. Перейдите в каталог с ролью kibana-role и создайте сценарий тестирования по умолчанию
```
iv_art@Pappa:/mnt/c/Users/iv_ar/Documents/GitHub/ansible_labs/ansible_yandex/playbook/roles/kibana-role$ molecule init scenario --driver-name docker
INFO     Initializing new scenario default...
INFO     Initialized scenario in /mnt/c/Users/iv_ar/Documents/GitHub/ansible_labs/ansible_yandex/playbook/roles/kibana-role/molecule/default successfully.
```
3. Для выполнения роли на centos 8 добавил файлы download_dnf, install_dnf,
получил типичную ошибку CentOS-Error Failed to download metadata. Добавил код:
```
- name: settin net1
  ansible.builtin.shell: |
    sudo -i
    sed -i 's/mirrorlist/#mirrorlist/g' *
    sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' *
  args:
    chdir: etc/yum.repos.d/
  register: net_cfg_changed
  changed_when: false
```
Итого:
```
RUNNING HANDLER [kibana-role : restart Kibana] *********************************
changed: [centos7]
changed: [centos8]
changed: [ubuntu]

PLAY RECAP *********************************************************************
centos7                    : ok=9    changed=5    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
centos8                    : ok=11   changed=5    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
el-instance                : ok=8    changed=4    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ubuntu                     : ok=9    changed=5    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
*
*
TASK [Example assertion] *******************************************************
ok: [centos7] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [centos8] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [el-instance] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos7                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
centos8                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
el-instance                : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
4. Добавлены проверки на прослушивание порта 5601 и присутствие лог-файла.
```
PLAY [Verify] ******************************************************************

TASK [Wait for Kibana port is listening] ***************************************
ok: [ubuntu]
ok: [centos8]
ok: [centos7]

TASK [Wait for Kibana log file created] ****************************************
ok: [centos7]
ok: [ubuntu]
ok: [centos8]

PLAY RECAP *********************************************************************
centos7                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
centos8                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
```
5. То же для filebeat
![Screenshot 2022-02-28 215459](https://user-images.githubusercontent.com/87374285/155979447-c7893abe-90ef-40b6-babb-ae92c4af9193.png)  

6. Зайти поочерёдно в каждую из них и запустите команду tox. Убедитесь, что всё отработало успешно.

```
[root@2912fb230b6c kibana-role]# tox
py36-ansible29 installed: ansible==2.9.27,ansible-compat==1.0.0,ansible-lint==5.4.0,arrow==1.2.2,bcrypt==3.2.0,binaryornot==0.4.4,bracex==2.2.1,cached-property==1.5.2,Cerberus==1.3.2,certifi==2021.10.8,cffi==1.15.0,chardet==4.0.0,charset-normalizer==2.0.12,click==8.0.4,click-help-colors==0.9.1,colorama==0.4.4,commonmark==0.9.1,cookiecutter==1.7.3,cryptography==36.0.1,dataclasses==0.8,distro==1.7.0,docker==5.0.3,enrich==1.2.7,idna==3.3,importlib-metadata==4.8.3,Jinja2==3.0.3,jinja2-time==0.2.0,MarkupSafe==2.0.1,molecule==3.4.0,molecule-docker==1.1.0,packaging==21.3,paramiko==2.9.2,pathspec==0.9.0,pluggy==0.13.1,poyo==0.5.0,pycparser==2.21,Pygments==2.11.2,PyNaCl==1.5.0,pyparsing==3.0.7,python-dateutil==2.8.2,python-slugify==6.1.1,PyYAML==5.4.1,requests==2.27.1,rich==11.2.0,ruamel.yaml==0.17.21,ruamel.yaml.clib==0.2.6,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.0.1,text-unidecode==1.3,typing_extensions==4.1.1,urllib3==1.26.8,wcmatch==8.3,websocket-client==1.3.1,yamllint==1.26.3,zipp==3.6.0
py36-ansible29 run-test-pre: PYTHONHASHSEED='62728206'
py36-ansible29 run-test: commands[0] | molecule test -s tox --destroy=always
INFO     tox scenario test matrix: destroy, create, converge, destroy
INFO     Performing prerun...
INFO     Guessed /opt/kibana-role as project root directory
INFO     Running ansible-galaxy role install --force --roles-path /root/.cache/ansible-lint/c5f5dd/roles -vr molecule/default/requirements.yml
INFO     Running ansible-galaxy role install --force --roles-path /root/.cache/ansible-lint/c5f5dd/roles -vr molecule/tox/requirements.yml
INFO     Using /root/.cache/ansible-lint/c5f5dd/roles/my_workspace.kibana_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/root/.cache/ansible-lint/c5f5dd/roles
INFO     Running tox > destroy
INFO     Sanity checks: 'docker'
**
```
repo:
https://github.com/iv-art074/ansible_labs/tree/testin/ansible_yandex/playbook/roles/kibana-role 
https://github.com/iv-art074/ansible_labs/tree/testin/ansible_yandex/playbook/roles/filebeat-role 
