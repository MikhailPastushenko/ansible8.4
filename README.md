# 8.2


```mikhail@Ubuntu1:~/PycharmProjects/ansible2$ ansible-playbook -i inventory/prod.yml site.yml --check

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

PLAY [Install Vector] ***********************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [vector-01]

TASK [Get vector rpm] ***********************************************************************************************************************
changed: [vector-01]

TASK [Install vector rpm] *******************************************************************************************************************
fatal: [vector-01]: FAILED! => {"changed": false, "msg": "No RPM file matching 'vector-0.24.0-1.x86_64.rpm' found on system", "rc": 127, "results": ["No RPM file matching 'vector-0.24.0-1.x86_64.rpm' found on system"]}

PLAY RECAP **********************************************************************************************************************************
clickhouse-01              : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
vector-01                  : ok=2    changed=1    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
```

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

PLAY [Install Vector] ***********************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************
ok: [vector-01]

TASK [Get vector rpm] ***********************************************************************************************************************
changed: [vector-01]

TASK [Install vector rpm] *******************************************************************************************************************
fatal: [vector-01]: FAILED! => {"changed": false, "changes": {"installed": ["vector-0.24.0-1.x86_64.rpm"]}, "msg": "Error: Package: vector-0.24.0-1.x86_64 (/vector-0.24.0-1.x86_64)\n           Requires: libstdc++.so.6(CXXABI_1.3.8)(64bit)\nError: Package: vector-0.24.0-1.x86_64 (/vector-0.24.0-1.x86_64)\n           Requires: libc.so.6(GLIBC_2.18)(64bit)\nError: Package: vector-0.24.0-1.x86_64 (/vector-0.24.0-1.x86_64)\n           Requires: libstdc++.so.6(GLIBCXX_3.4.21)(64bit)\n", "rc": 1, "results": ["Loaded plugins: fastestmirror\nExamining vector-0.24.0-1.x86_64.rpm: vector-0.24.0-1.x86_64\nMarking vector-0.24.0-1.x86_64.rpm to be installed\nResolving Dependencies\n--> Running transaction check\n---> Package vector.x86_64 0:0.24.0-1 will be installed\n--> Processing Dependency: libc.so.6(GLIBC_2.18)(64bit) for package: vector-0.24.0-1.x86_64\nLoading mirror speeds from cached hostfile\n * base: centos-mirror.rbc.ru\n * extras: mirror.awanti.com\n * updates: mirror.awanti.com\n--> Processing Dependency: libstdc++.so.6(CXXABI_1.3.8)(64bit) for package: vector-0.24.0-1.x86_64\n--> Processing Dependency: libstdc++.so.6(GLIBCXX_3.4.21)(64bit) for package: vector-0.24.0-1.x86_64\n--> Finished Dependency Resolution\n You could try using --skip-broken to work around the problem\n You could try running: rpm -Va --nofiles --nodigest\n"]}

PLAY RECAP **********************************************************************************************************************************
clickhouse-01              : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
vector-01                  : ok=2    changed=1    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   

```