# Задание 8.2

В site.yaml добавлен Play "Install Vector" с двумя тасками "Get vector rpm" и "Install vector rpm".

В group_vars добавлена переменная для версии vector

В inventory добавлны группы хостов , в которых указан развернутый хост.  

#### Запуск с ключом --check  ####

```mikhail@Ubuntu1:~/PycharmProjects/ansible2$ ansible-playbook -i inventory/prod.yml site.yml --check

PLAY [Install Clickhouse] *******************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ***************************************************************************************************************
changed: [clickhouse-01] => (item=clickhouse-client)
changed: [clickhouse-01] => (item=clickhouse-server)
failed: [clickhouse-01] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "item": "clickhouse-common-static", "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ***************************************************************************************************************
changed: [clickhouse-01]

TASK [Install clickhouse packages] **********************************************************************************************************
fatal: [clickhouse-01]: FAILED! => {"changed": false, "msg": "No RPM file matching 'clickhouse-common-static-22.3.3.44.rpm' found on system", "rc": 127, "results": ["No RPM file matching 'clickhouse-common-static-22.3.3.44.rpm' found on system"]}
...ignoring

TASK [Create database] **********************************************************************************************************************
skipping: [clickhouse-01]

PLAY [Install Vector] ***********************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [vector-01]

TASK [Get vector rpm] ***********************************************************************************************************************
changed: [vector-01]

TASK [Install vector rpm] *******************************************************************************************************************
fatal: [vector-01]: FAILED! => {"changed": false, "msg": "No RPM file matching 'vector-0.24.0-1.x86_64.rpm' found on system", "rc": 127, "results": ["No RPM file matching 'vector-0.24.0-1.x86_64.rpm' found on system"]}

PLAY RECAP **********************************************************************************************************************************
clickhouse-01              : ok=3    changed=1    unreachable=0    failed=0    skipped=1    rescued=1    ignored=1   
vector-01                  : ok=2    changed=1    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   

```

#### Запуск с ключом --diff ####

```mikhail@Ubuntu1:~/PycharmProjects/ansible2$ ansible-playbook -i inventory/prod.yml site.yml --diff

PLAY [Install Clickhouse] *******************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [clickhouse-01]

TASK [Get clickhouse distrib] ***************************************************************************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
failed: [clickhouse-01] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 0, "group": "root", "item": "clickhouse-common-static", "mode": "0644", "msg": "Request failed", "owner": "root", "response": "HTTP Error 404: Not Found", "secontext": "unconfined_u:object_r:admin_home_t:s0", "size": 246310036, "state": "file", "status_code": 404, "uid": 0, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ***************************************************************************************************************
ok: [clickhouse-01]

TASK [Install clickhouse packages] **********************************************************************************************************
ok: [clickhouse-01]

TASK [Create database] **********************************************************************************************************************
changed: [clickhouse-01]

PLAY [Install Vector] ***********************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [vector-01]

TASK [Get vector rpm] ***********************************************************************************************************************
changed: [vector-01]

TASK [Install vector rpm] *******************************************************************************************************************
changed: [vector-01]

PLAY RECAP **********************************************************************************************************************************
clickhouse-01              : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
vector-01                  : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
  
```
#### Запуск с ключом --tags ####

```mikhail@Ubuntu1:~/PycharmProjects/ansible2$ ansible-playbook -i inventory/prod.yml site.yml --tags vector

PLAY [Install Clickhouse] *******************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [clickhouse-01]

PLAY [Install Vector] ***********************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [vector-01]

TASK [Get vector rpm] ***********************************************************************************************************************
ok: [vector-01]

TASK [Install vector rpm] *******************************************************************************************************************
ok: [vector-01]

PLAY RECAP **********************************************************************************************************************************
clickhouse-01              : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-01                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

