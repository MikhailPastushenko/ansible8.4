# 8.2

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
changed: [clickhouse-01] => (item=clickhouse-client)
changed: [clickhouse-01] => (item=clickhouse-server)
failed: [clickhouse-01] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "item": "clickhouse-common-static", "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ***************************************************************************************************************
changed: [clickhouse-01]

TASK [Install clickhouse packages] **********************************************************************************************************
changed: [clickhouse-01]

TASK [Create database] **********************************************************************************************************************
fatal: [clickhouse-01]: FAILED! => {"changed": false, "cmd": ["clickhouse-client", "-q", "create database logs;"], "delta": "0:00:00.027523", "end": "2022-09-15 20:45:10.651918", "failed_when_result": true, "msg": "non-zero return code", "rc": 210, "start": "2022-09-15 20:45:10.624395", "stderr": "Code: 210. DB::NetException: Connection refused (localhost:9000). (NETWORK_ERROR)", "stderr_lines": ["Code: 210. DB::NetException: Connection refused (localhost:9000). (NETWORK_ERROR)"], "stdout": "", "stdout_lines": []}
...ignoring

RUNNING HANDLER [Start clickhouse service] **************************************************************************************************
changed: [clickhouse-01]

PLAY [Install Vector] ***********************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [vector-01]

TASK [Get vector rpm] ***********************************************************************************************************************
changed: [vector-01]

TASK [Install vector rpm] *******************************************************************************************************************
fatal: [vector-01]: FAILED! => {"changed": false, "changes": {"installed": ["vector-0.24.0-1.x86_64.rpm"]}, "msg": "Error: Package: vector-0.24.0-1.x86_64 (/vector-0.24.0-1.x86_64)\n           Requires: libstdc++.so.6(CXXABI_1.3.8)(64bit)\nError: Package: vector-0.24.0-1.x86_64 (/vector-0.24.0-1.x86_64)\n           Requires: libc.so.6(GLIBC_2.18)(64bit)\nError: Package: vector-0.24.0-1.x86_64 (/vector-0.24.0-1.x86_64)\n           Requires: libstdc++.so.6(GLIBCXX_3.4.21)(64bit)\n", "rc": 1, "results": ["Loaded plugins: fastestmirror, langpacks\nExamining vector-0.24.0-1.x86_64.rpm: vector-0.24.0-1.x86_64\nMarking vector-0.24.0-1.x86_64.rpm to be installed\nResolving Dependencies\n--> Running transaction check\n---> Package vector.x86_64 0:0.24.0-1 will be installed\n--> Processing Dependency: libc.so.6(GLIBC_2.18)(64bit) for package: vector-0.24.0-1.x86_64\nLoading mirror speeds from cached hostfile\n * base: mirror.docker.ru\n * extras: mirror.yandex.ru\n * updates: mirror.yandex.ru\n--> Processing Dependency: libstdc++.so.6(CXXABI_1.3.8)(64bit) for package: vector-0.24.0-1.x86_64\n--> Processing Dependency: libstdc++.so.6(GLIBCXX_3.4.21)(64bit) for package: vector-0.24.0-1.x86_64\n--> Finished Dependency Resolution\n You could try using --skip-broken to work around the problem\n You could try running: rpm -Va --nofiles --nodigest\n"]}

PLAY RECAP **********************************************************************************************************************************
clickhouse-01              : ok=5    changed=3    unreachable=0    failed=0    skipped=0    rescued=1    ignored=1   
vector-01                  : ok=2    changed=1    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
```

К сожалению , сам пакет vector не установился из-за зависимостей 
