### Домашнее задание к занятию 5 «Тестирование roles»

### Подготовка к выполнению

1. Установите molecule: `pip3 install "molecule==3.5.2"` и драйвера `pip3 install molecule_docker molecule_podman`.

2. Выполните `docker pull aragast/netology:latest` —  это образ с podman, tox и несколькими пайтонами (3.7 и 3.9) внутри.

## Основная часть

Ваша цель — настроить тестирование ваших ролей. 

Задача — сделать сценарии тестирования для vector. 

Ожидаемый результат — все сценарии успешно проходят тестирование ролей.

### Molecule

1. Запустите  `molecule test -s centos_7` внутри корневой директории clickhouse-role, посмотрите на вывод команды. Данная команда может отработать с ошибками, это нормально. Наша цель - посмотреть как другие в реальном мире используют молекулу.

Используемое окружение:

```
root@baranovsa:/home/baranovsa/8.5_testing/playbook/roles/vector-role# molecule --version
molecule 6.0.2 using python 3.9 
    ansible:2.15.8
    azure:23.5.0 from molecule_plugins
    containers:23.5.0 from molecule_plugins requiring collections: ansible.posix>=1.3.0 community.docker>=1.9.1 containers.podman>=1.8.1
    default:6.0.2 from molecule
    docker:23.5.0 from molecule_plugins requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0
    ec2:23.5.0 from molecule_plugins
    gce:23.5.0 from molecule_plugins requiring collections: google.cloud>=1.0.2 community.crypto>=1.8.0
    podman:23.5.0 from molecule_plugins requiring collections: containers.podman>=1.7.0 ansible.posix>=1.3.0
    vagrant:23.5.0 from molecule_plugins
root@baranovsa:/home/baranovsa/8.5_testing/playbook/roles/vector-role# 
```

2. Перейдите в каталог с ролью vector-role и создайте сценарий тестирования по умолчанию при помощи `molecule init scenario --driver-name docker`.

```
/home/baranovsa/8.5_testing/playbook/roles/vector-role# molecule init scenario --driver-name docker
INFO     Initializing new scenario default...
INFO     Initialized scenario in /home/sa/vector-role/molecule/default successfully.
/home/baranovsa/8.5_testing/playbook/roles/vector-role#
```

<details><summary>Вывод команды molecule test</summary>

```
root@baranovsa:/home/baranovsa/8.5_testing/playbook/roles/vector-role# molecule test
WARNING  Driver docker does not provide a schema.
INFO 	default scenario test matrix: dependency, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO 	Performing prerun with role_name_check=0...
INFO 	Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/f5bcd7/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO 	Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/f5bcd7/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO 	Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/f5bcd7/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO 	Using /root/.cache/ansible-compat/f5bcd7/roles/sergey.vector symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO 	Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO 	Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO 	Running default > destroy
INFO 	Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos7)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
ok: [localhost] => (item=centos7)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost              	: ok=3	changed=1	unreachable=0	failed=0	skipped=1	rescued=0	ignored=0

INFO 	Running default > syntax

playbook: /home/baranovsa/8.5_testing/playbook/roles/vector-role/molecule/default/converge.yml
INFO 	Running default > create

PLAY [Create] ******************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost]

TASK [Synchronization the context] *********************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost]

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)
skipping: [localhost]

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos7)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j132983609593.29828', 'results_file': '/root/.ansible_async/j132983609593.29828', 'changed': True, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost              	: ok=6	changed=2	unreachable=0	failed=0	skipped=5	rescued=0	ignored=0

INFO 	Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO 	Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]

TASK [Detect python3 version] **************************************************
ok: [centos7]

TASK [Set python version of script to 3] ***************************************
skipping: [centos7]

TASK [Download SystemD replacer v3] ********************************************
skipping: [centos7]

TASK [Download SystemD replacer v2] ********************************************
changed: [centos7]

TASK [Create systemd directories] **********************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
 	"path": "/run/systemd/system",
-	"state": "absent"
+	"state": "directory"
 }

changed: [centos7] => (item=/run/systemd/system)
ok: [centos7] => (item=/usr/lib/systemd/system)

TASK [Replace systemctl] *******************************************************
changed: [centos7]

TASK [ReCollect system info] ***************************************************
ok: [centos7]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Get vector distrib] ****************************************
changed: [centos7]

TASK [vector-role : Install vector package] ************************************
changed: [centos7]

TASK [vector-role : Redefine vector config name] *******************************
--- before: /etc/default/vector (content)
+++ after: /etc/default/vector (content)
@@ -2,3 +2,4 @@
 # This file can theoretically contain a bunch of environment variables
 # for Vector.  See https://vector.dev/docs/setup/configuration/#environment-variables
 # for details.
+VECTOR_CONFIG=/etc/vector/config.yaml

changed: [centos7]

TASK [vector-role : Create vector config] **************************************
--- before
+++ after: /root/.ansible/tmp/ansible-local-299635no23q6d/tmpjxuwu3s1
@@ -0,0 +1,27 @@
+api:
+  address: 0.0.0.0:8686
+  enabled: true
+sinks:
+  to_clickhouse:
+	compression: gzip
+	database: vector_logs
+	endpoint: http://178.154.204.25:8123
+	inputs:
+	- parse_logs
+	table: logs_logs
+	type: clickhouse
+sources:
+  logs_logs:
+	format: syslog
+	interval: 1
+	type: demo_logs
+transforms:
+  parse_logs:
+	inputs:
+	- logs_logs
+	source: '. = parse_syslog!(string!(.message))
+
+  	.timestamp = to_string(.timestamp)
+
+  	.timestamp = slice!(.timestamp, start:0, end: -1)'
+	type: remap

changed: [centos7]

TASK [vector-role : Flush handlers] ********************************************

PLAY RECAP *********************************************************************
centos7                	: ok=10   changed=7	unreachable=0	failed=0	skipped=2	rescued=0	ignored=0

INFO 	Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]

TASK [Detect python3 version] **************************************************
ok: [centos7]

TASK [Set python version of script to 3] ***************************************
skipping: [centos7]

TASK [Download SystemD replacer v3] ********************************************
skipping: [centos7]

TASK [Download SystemD replacer v2] ********************************************
ok: [centos7]

TASK [Create systemd directories] **********************************************
ok: [centos7] => (item=/run/systemd/system)
ok: [centos7] => (item=/usr/lib/systemd/system)

TASK [Replace systemctl] *******************************************************
ok: [centos7]

TASK [ReCollect system info] ***************************************************
ok: [centos7]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Get vector distrib] ****************************************
ok: [centos7]

TASK [vector-role : Install vector package] ************************************
ok: [centos7]

TASK [vector-role : Redefine vector config name] *******************************
ok: [centos7]

TASK [vector-role : Create vector config] **************************************
ok: [centos7]

TASK [vector-role : Flush handlers] ********************************************

PLAY RECAP *********************************************************************
centos7                	: ok=10   changed=0	unreachable=0	failed=0	skipped=2	rescued=0	ignored=0

INFO 	Idempotence completed successfully.
INFO 	Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO 	Running default > verify
INFO 	Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [centos7] => {
	"changed": false,
	"msg": "All assertions passed"
}

TASK [Get Vector version] ******************************************************
ok: [centos7]

TASK [Assert Vector instalation] ***********************************************
ok: [centos7] => {
	"changed": false,
	"msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos7                	: ok=3	changed=0	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0

INFO 	Verifier completed successfully.
INFO 	Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO 	Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos7)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=centos7)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost              	: ok=3	changed=2	unreachable=0	failed=0	skipped=1	rescued=0	ignored=0

INFO 	Pruning extra files from scenario ephemeral directory
root@baranovsa:/home/baranovsa/8.5_testing/playbook/roles/vector-role#


```

</details>


3. Добавьте несколько разных дистрибутивов (centos:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.

Для работы в Docker были использованы готовые образы за авторством pycontribs:

CentOS (доступные теги: 7) - docker.io/pycontribs/centos

Итоговый файл настройки molecule:

```
---
dependency:
  name: galaxy
driver:
  name: docker
  options:
    D: true
    vv: true
lint: |
  ansible-lint .
  yamllint .
platforms:
  - name: centos7
    image: docker.io/pycontribs/centos:7
    pre_build_image: true
provisioner:
  name: ansible
  options:
    D: true
    vv: true
verifier:
  name: ansible
```

4. Добавьте несколько assert в verify.yml-файл для  проверки работоспособности vector-role (проверка, что конфиг валидный, проверка успешности запуска и др.). 

Готовый playbook проверок:

```
---
# This is an example playbook to execute Ansible tests.

- name: Verify
  hosts: all
  gather_facts: false
  tasks:
  - name: Example assertion
    ansible.builtin.assert:
      that: true
  - name: Get Vector version
    ansible.builtin.command: "vector --version"
    changed_when: false
    register: vector_version
  - name: Assert Vector instalation
    assert:
      that: "'{{ vector_version.rc }}' == '0'"
```

5. Запустите тестирование роли повторно и проверьте, что оно прошло успешно.

<details><summary>Вывод команды molecule test</summary>

```
root@baranovsa:/home/baranovsa/8.5_testing/playbook/roles/vector-role# molecule test
WARNING  Driver docker does not provide a schema.
INFO 	default scenario test matrix: dependency, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO 	Performing prerun with role_name_check=0...
INFO 	Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/f5bcd7/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO 	Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/f5bcd7/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO 	Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/f5bcd7/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO 	Using /root/.cache/ansible-compat/f5bcd7/roles/sergey.vector symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO 	Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO 	Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO 	Running default > destroy
INFO 	Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos7)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
ok: [localhost] => (item=centos7)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost              	: ok=3	changed=1	unreachable=0	failed=0	skipped=1	rescued=0	ignored=0

INFO 	Running default > syntax

playbook: /home/baranovsa/8.5_testing/playbook/roles/vector-role/molecule/default/converge.yml
INFO 	Running default > create

PLAY [Create] ******************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost]

TASK [Synchronization the context] *********************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost]

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)
skipping: [localhost]

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos7)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j374428537465.34656', 'results_file': '/root/.ansible_async/j374428537465.34656', 'changed': True, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost              	: ok=6	changed=2	unreachable=0	failed=0	skipped=5	rescued=0	ignored=0

INFO 	Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO 	Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]

TASK [Detect python3 version] **************************************************
ok: [centos7]

TASK [Set python version of script to 3] ***************************************
skipping: [centos7]

TASK [Download SystemD replacer v3] ********************************************
skipping: [centos7]

TASK [Download SystemD replacer v2] ********************************************
changed: [centos7]

TASK [Create systemd directories] **********************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
 	"path": "/run/systemd/system",
-	"state": "absent"
+	"state": "directory"
 }

changed: [centos7] => (item=/run/systemd/system)
ok: [centos7] => (item=/usr/lib/systemd/system)

TASK [Replace systemctl] *******************************************************
changed: [centos7]

TASK [ReCollect system info] ***************************************************
ok: [centos7]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Get vector distrib] ****************************************
changed: [centos7]

TASK [vector-role : Install vector package] ************************************
changed: [centos7]

TASK [vector-role : Redefine vector config name] *******************************
--- before: /etc/default/vector (content)
+++ after: /etc/default/vector (content)
@@ -2,3 +2,4 @@
 # This file can theoretically contain a bunch of environment variables
 # for Vector.  See https://vector.dev/docs/setup/configuration/#environment-variables
 # for details.
+VECTOR_CONFIG=/etc/vector/config.yaml

changed: [centos7]

TASK [vector-role : Create vector config] **************************************
--- before
+++ after: /root/.ansible/tmp/ansible-local-34797pqdf0fw2/tmpg91wqt38
@@ -0,0 +1,27 @@
+api:
+  address: 0.0.0.0:8686
+  enabled: true
+sinks:
+  to_clickhouse:
+	compression: gzip
+	database: vector_logs
+	endpoint: http://178.154.204.25:8123
+	inputs:
+	- parse_logs
+	table: logs_logs
+	type: clickhouse
+sources:
+  logs_logs:
+	format: syslog
+	interval: 1
+	type: demo_logs
+transforms:
+  parse_logs:
+	inputs:
+	- logs_logs
+	source: '. = parse_syslog!(string!(.message))
+
+  	.timestamp = to_string(.timestamp)
+
+  	.timestamp = slice!(.timestamp, start:0, end: -1)'
+	type: remap

changed: [centos7]

TASK [vector-role : Flush handlers] ********************************************

PLAY RECAP *********************************************************************
centos7                	: ok=10   changed=7	unreachable=0	failed=0	skipped=2	rescued=0	ignored=0

INFO 	Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]

TASK [Detect python3 version] **************************************************
ok: [centos7]

TASK [Set python version of script to 3] ***************************************
skipping: [centos7]

TASK [Download SystemD replacer v3] ********************************************
skipping: [centos7]

TASK [Download SystemD replacer v2] ********************************************
ok: [centos7]

TASK [Create systemd directories] **********************************************
ok: [centos7] => (item=/run/systemd/system)
ok: [centos7] => (item=/usr/lib/systemd/system)

TASK [Replace systemctl] *******************************************************
ok: [centos7]

TASK [ReCollect system info] ***************************************************
ok: [centos7]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Get vector distrib] ****************************************
ok: [centos7]

TASK [vector-role : Install vector package] ************************************
ok: [centos7]

TASK [vector-role : Redefine vector config name] *******************************
ok: [centos7]

TASK [vector-role : Create vector config] **************************************
ok: [centos7]

TASK [vector-role : Flush handlers] ********************************************

PLAY RECAP *********************************************************************
centos7                	: ok=10   changed=0	unreachable=0	failed=0	skipped=2	rescued=0	ignored=0

INFO 	Idempotence completed successfully.
INFO 	Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO 	Running default > verify
INFO 	Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [centos7] => {
	"changed": false,
	"msg": "All assertions passed"
}

TASK [Get Vector version] ******************************************************
ok: [centos7]

TASK [Assert Vector instalation] ***********************************************
ok: [centos7] => {
	"changed": false,
	"msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos7                	: ok=3	changed=0	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0

INFO 	Verifier completed successfully.
INFO 	Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO 	Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos7)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=centos7)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost              	: ok=3	changed=2	unreachable=0	failed=0	skipped=1	rescued=0	ignored=0

INFO 	Pruning extra files from scenario ephemeral directory
root@baranovsa:/home/baranovsa/8.5_testing/playbook/roles/vector-role#

```

</details>


5. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

[1.1.0](https://github.com/12sergey12/vector-role/releases/tag/1.1.0)

### Tox

1. Добавьте в директорию с vector-role файлы из [директории](./example).

2. Запустите `docker run --privileged=True -v <path_to_repo>:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash`, где path_to_repo — путь до корня репозитория с
 vector-role на вашей файловой системе.

```
root@baranovsa:/home/baranovsa/8.5_testing/playbook/roles/vector-role# docker run --privileged=True -v `pwd`:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash
[root@903d6353cd1b vector-role]# tox
py37-ansible210 create: /opt/vector-role/.tox/py37-ansible210
py37-ansible210 installdeps: -rtox-requirements.txt, ansible<3.0
py37-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.1.1,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.5,certifi==2023.11.17,cffi==1.15.1,chardet==5.2.0,charset-normalizer==3.3.2,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.5.0,cryptography==41.0.7,distro==1.8.0,enrich==1.2.7,idna==3.6,importlib-metadata==6.7.0,Jinja2==3.1.2,jmespath==1.0.1,lxml==4.9.3,markdown-it-py==2.2.0,MarkupSafe==2.1.3,mdurl==0.1.2,molecule==3.5.2,molecule-podman==1.1.0,packaging==23.2,paramiko==2.12.0,pathspec==0.11.2,pluggy==1.2.0,pycparser==2.21,Pygments==2.17.2,PyNaCl==1.5.0,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==5.4.1,requests==2.31.0,rich==13.7.0,ruamel.yaml==0.18.5,ruamel.yaml.clib==0.2.8,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.2.3,text-unidecode==1.3,typing_extensions==4.7.1,urllib3==2.0.7,wcmatch==8.4.1,yamllint==1.26.3,zipp==3.15.0
py37-ansible210 run-test-pre: PYTHONHASHSEED='3426873990'
py37-ansible210 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py37-ansible210/bin/molecule test -s compatibility --destroy always (exited with code 1)
py37-ansible30 create: /opt/vector-role/.tox/py37-ansible30
py37-ansible30 installdeps: -rtox-requirements.txt, ansible<3.1
py37-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.1.1,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.5,certifi==2023.11.17,cffi==1.15.1,chardet==5.2.0,charset-normalizer==3.3.2,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.5.0,cryptography==41.0.7,distro==1.8.0,enrich==1.2.7,idna==3.6,importlib-metadata==6.7.0,Jinja2==3.1.2,jmespath==1.0.1,lxml==4.9.3,markdown-it-py==2.2.0,MarkupSafe==2.1.3,mdurl==0.1.2,molecule==3.5.2,molecule-podman==1.1.0,packaging==23.2,paramiko==2.12.0,pathspec==0.11.2,pluggy==1.2.0,pycparser==2.21,Pygments==2.17.2,PyNaCl==1.5.0,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==5.4.1,requests==2.31.0,rich==13.7.0,ruamel.yaml==0.18.5,ruamel.yaml.clib==0.2.8,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.2.3,text-unidecode==1.3,typing_extensions==4.7.1,urllib3==2.0.7,wcmatch==8.4.1,yamllint==1.26.3,zipp==3.15.0
py37-ansible30 run-test-pre: PYTHONHASHSEED='3426873990'
py37-ansible30 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py37-ansible30/bin/molecule test -s compatibility --destroy always (exited with code 1)
py39-ansible210 create: /opt/vector-role/.tox/py39-ansible210
py39-ansible210 installdeps: -rtox-requirements.txt, ansible<3.0
py39-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==4.1.10,ansible-core==2.15.7,ansible-lint==5.1.3,arrow==1.3.0,attrs==23.1.0,bcrypt==4.1.1,binaryornot==0.4.4,bracex==2.4,Cerberus==1.3.5,certifi==2023.11.17,cffi==1.16.0,chardet==5.2.0,charset-normalizer==3.3.2,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.5.0,cryptography==41.0.7,distro==1.8.0,enrich==1.2.7,idna==3.6,importlib-resources==5.0.7,Jinja2==3.1.2,jmespath==1.0.1,jsonschema==4.20.0,jsonschema-specifications==2023.11.2,lxml==4.9.3,markdown-it-py==3.0.0,MarkupSafe==2.1.3,mdurl==0.1.2,molecule==3.5.2,molecule-podman==2.0.0,packaging==23.2,paramiko==2.12.0,pathspec==0.12.1,pluggy==1.3.0,pycparser==2.21,Pygments==2.17.2,PyNaCl==1.5.0,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==5.4.1,referencing==0.32.0,requests==2.31.0,resolvelib==1.0.1,rich==13.7.0,rpds-py==0.13.2,ruamel.yaml==0.18.5,ruamel.yaml.clib==0.2.8,selinux==0.3.0,six==1.16.0,subprocess-tee==0.4.1,tenacity==8.2.3,text-unidecode==1.3,types-python-dateutil==2.8.19.14,typing_extensions==4.9.0,urllib3==2.1.0,wcmatch==8.5,yamllint==1.26.3
py39-ansible210 run-test-pre: PYTHONHASHSEED='3426873990'
39-ansible210 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py39-ansible210/bin/molecule test -s compatibility --destroy always (exited with code 1)
py39-ansible30 create: /opt/vector-role/.tox/py39-ansible30
py39-ansible30 installdeps: -rtox-requirements.txt, ansible<3.1
py39-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==4.1.10,ansible-core==2.15.7,ansible-lint==5.1.3,arrow==1.3.0,attrs==23.1.0,bcrypt==4.1.1,binaryornot==0.4.4,bracex==2.4,Cerberus==1.3.5,certifi==2023.11.17,cffi==1.16.0,chardet==5.2.0,charset-normalizer==3.3.2,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.5.0,cryptography==41.0.7,distro==1.8.0,enrich==1.2.7,idna==3.6,importlib-resources==5.0.7,Jinja2==3.1.2,jmespath==1.0.1,jsonschema==4.20.0,jsonschema-specifications==2023.11.2,lxml==4.9.3,markdown-it-py==3.0.0,MarkupSafe==2.1.3,mdurl==0.1.2,molecule==3.5.2,molecule-podman==2.0.0,packaging==23.2,paramiko==2.12.0,pathspec==0.12.1,pluggy==1.3.0,pycparser==2.21,Pygments==2.17.2,PyNaCl==1.5.0,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==5.4.1,referencing==0.32.0,requests==2.31.0,resolvelib==1.0.1,rich==13.7.0,rpds-py==0.13.2,ruamel.yaml==0.18.5,ruamel.yaml.clib==0.2.8,selinux==0.3.0,six==1.16.0,subprocess-tee==0.4.1,tenacity==8.2.3,text-unidecode==1.3,types-python-dateutil==2.8.19.14,typing_extensions==4.9.0,urllib3==2.1.0,wcmatch==8.5,yamllint==1.26.3
py39-ansible30 run-test-pre: PYTHONHASHSEED='3426873990'
py39-ansible30 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py39-ansible30/bin/molecule test -s compatibility --destroy always (exited with code 1)
________________________________________________ summary ________________________________________________
ERROR:   py37-ansible210: commands failed
ERROR:   py37-ansible30: commands failed
ERROR:   py39-ansible210: commands failed
ERROR:   py39-ansible30: commands failed
[root@903d6353cd1b vector-role]#
```
Из сообщений видно, что все проверки Tox завершились одинаковой ошибкой, связанной с исполняемой командой, а именно: molecule test -s compatibility --destroy always, так как сценария compatibility в роли нет.


3. Внутри контейнера выполните команду `tox`, посмотрите на вывод.
5. Создайте облегчённый сценарий для `molecule` с драйвером `molecule_podman`. Проверьте его на исполнимость.

[molecule.yml](https://github.com/12sergey12/08-ansible-05-testing/blob/main/playbook/roles/vector-role/molecule/podman/molecule.yml)

```
---
dependency:
  name: galaxy
driver:
  name: podman
  options:
    D: true
    vv: true
platforms:
  - name: centos7
    image: docker.io/pycontribs/centos:7
    pre_build_image: true
#  - name: centos8
#    image: docker.io/pycontribs/centos:8
#    pre_build_image: true
#  - name: ubuntu
#    image: docker.io/pycontribs/ubuntu:latest
#    pre_build_image: true
provisioner:
  name: ansible
  options:
    D: true
    vv: true
verifier:
  name: ansible
scenario:
  create_sequence:
    - create
  check_sequence:
    - check
  converge_sequence:
    - converge
  destroy_sequence:
    - destroy
  test_sequence:
    - create
    - converge
    - verify
    - destroy
```

6. Пропишите правильную команду в `tox.ini`, чтобы запускался облегчённый сценарий.

[tox.ini](https://github.com/12sergey12/08-ansible-05-testing/blob/main/playbook/roles/vector-role/tox.ini)

```
[tox]
minversion = 1.8
basepython = python3.9
envlist = py{37}-ansible{210,30}
skipsdist = true

[testenv]
passenv = *
deps =
    -r tox-requirements.txt
    ansible210: ansible<3.0
    ansible30: ansible<3.1
commands =
    {posargs:molecule test -s podman --destroy always}
```

8. Запустите команду `tox`. Убедитесь, что всё отработало успешно.

<details><summary>Вывод команды tox</summary>

```
root@baranovsa:/home/baranovsa/8.5_testing/playbook/roles/vector-role# docker run --privileged=True -v `pwd`:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash
[root@952dc91470f6 vector-role]# tox
py37-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.1.1,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.5,certifi==2023.11.17,cffi==1.15.1,chardet==5.2.0,charset-normalizer==3.3.2,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.5.0,cryptography==41.0.7,distro==1.8.0,enrich==1.2.7,idna==3.6,importlib-metadata==6.7.0,Jinja2==3.1.2,jmespath==1.0.1,lxml==4.9.3,markdown-it-py==2.2.0,MarkupSafe==2.1.3,mdurl==0.1.2,molecule==3.5.2,molecule-podman==1.1.0,packaging==23.2,paramiko==2.12.0,pathspec==0.11.2,pluggy==1.2.0,pycparser==2.21,Pygments==2.17.2,PyNaCl==1.5.0,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==5.4.1,requests==2.31.0,rich==13.7.0,ruamel.yaml==0.18.5,ruamel.yaml.clib==0.2.8,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.2.3,text-unidecode==1.3,typing_extensions==4.7.1,urllib3==2.0.7,wcmatch==8.4.1,yamllint==1.26.3,zipp==3.15.0
py37-ansible210 run-test-pre: PYTHONHASHSEED='3731619642'
py37-ansible210 run-test: commands[0] | molecule test -s podman --destroy always
INFO 	podman scenario test matrix: create, converge, verify, destroy
INFO 	Performing prerun...
INFO 	Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/b984a4/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO 	Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/b984a4/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO 	Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/b984a4/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO 	Running podman > create
INFO 	Sanity checks: 'podman'

PLAY [Create] ******************************************************************

TASK [get podman executable path] **********************************************
ok: [localhost]

TASK [save path to executable as fact] *****************************************
ok: [localhost]

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item="centos7 registry username: None specified")

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item=Dockerfile: None specified)

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item="Dockerfile: None specified; Image: docker.io/pycontribs/centos:7")

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item=centos7)

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item=docker.io/pycontribs/centos:7)

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item="centos7 command: None specified")

TASK [Remove possible pre-existing containers] *********************************
changed: [localhost]

TASK [Discover local podman networks] ******************************************
skipping: [localhost] => (item=centos7: None specified)

TASK [Create podman network dedicated to this scenario] ************************
skipping: [localhost]

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos7)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (299 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (298 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (297 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (296 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (295 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (294 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (293 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (292 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (291 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (290 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (289 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (288 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (287 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (286 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (285 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (284 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (283 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (282 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (281 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (280 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (279 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (278 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (277 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (276 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (275 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (274 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (273 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (272 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (271 retries left).
changed: [localhost] => (item=centos7)

PLAY RECAP *********************************************************************
localhost              	: ok=8	changed=3	unreachable=0	failed=0	skipped=5	rescued=0	ignored=0

INFO 	Running podman > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]

TASK [Copy something to test use of synchronize module] ************************
--- before
+++ after: /etc/hosts
@@ -0,0 +1,7 @@
+127.0.0.1  	localhost
+::1	localhost ip6-localhost ip6-loopback
+fe00::0    	ip6-localnet
+ff00::0    	ip6-mcastprefix
+ff02::1    	ip6-allnodes
+ff02::2    	ip6-allrouters
+172.17.0.2 	952dc91470f6

changed: [centos7]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Get vector distrib] ****************************************
changed: [centos7]

TASK [vector-role : Install vector package] ************************************
changed: [centos7]

TASK [vector-role : Redefine vector config name] *******************************
--- before: /etc/default/vector (content)
+++ after: /etc/default/vector (content)
@@ -2,3 +2,4 @@
 # This file can theoretically contain a bunch of environment variables
 # for Vector.  See https://vector.dev/docs/setup/configuration/#environment-variables
 # for details.
+VECTOR_CONFIG=/etc/vector/config.yaml

changed: [centos7]

TASK [vector-role : Create vector config] **************************************
--- before
+++ after: /root/.ansible/tmp/ansible-local-718ag_kunlc/tmp661y66ah
@@ -0,0 +1,27 @@
+api:
+  address: 0.0.0.0:8686
+  enabled: true
+sinks:
+  to_clickhouse:
+	compression: gzip
+	database: vector_logs
+	endpoint: http://178.154.204.25:8123
+	inputs:
+	- parse_logs
+	table: logs_logs
+	type: clickhouse
+sources:
+  logs_logs:
+	format: syslog
+	interval: 1
+	type: demo_logs
+transforms:
+  parse_logs:
+	inputs:
+	- logs_logs
+	source: '. = parse_syslog!(string!(.message))
+
+  	.timestamp = to_string(.timestamp)
+
+  	.timestamp = slice!(.timestamp, start:0, end: -1)'
+	type: remap

changed: [centos7]

PLAY RECAP *********************************************************************
centos7                	: ok=6	changed=5	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0

INFO 	Running podman > verify
INFO 	Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [centos7] => {
	"changed": false,
	"msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos7                	: ok=1	changed=0	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0

INFO 	Verifier completed successfully.
INFO 	Running podman > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (299 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (298 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '801277396756.1992', 'results_file': '/root/.ansible_async/801277396756.1992', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost              	: ok=2	changed=2	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0

INFO 	Pruning extra files from scenario ephemeral directory
py37-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.1.1,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.5,certifi==2023.11.17,cffi==1.15.1,chardet==5.2.0,charset-normalizer==3.3.2,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.5.0,cryptography==41.0.7,distro==1.8.0,enrich==1.2.7,idna==3.6,importlib-metadata==6.7.0,Jinja2==3.1.2,jmespath==1.0.1,lxml==4.9.3,markdown-it-py==2.2.0,MarkupSafe==2.1.3,mdurl==0.1.2,molecule==3.5.2,molecule-podman==1.1.0,packaging==23.2,paramiko==2.12.0,pathspec==0.11.2,pluggy==1.2.0,pycparser==2.21,Pygments==2.17.2,PyNaCl==1.5.0,python-dateutil==2.8.2,python-slugify==8.0.1,PyYAML==5.4.1,requests==2.31.0,rich==13.7.0,ruamel.yaml==0.18.5,ruamel.yaml.clib==0.2.8,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.2.3,text-unidecode==1.3,typing_extensions==4.7.1,urllib3==2.0.7,wcmatch==8.4.1,yamllint==1.26.3,zipp==3.15.0
py37-ansible30 run-test-pre: PYTHONHASHSEED='3731619642'
py37-ansible30 run-test: commands[0] | molecule test -s podman --destroy always
INFO 	podman scenario test matrix: create, converge, verify, destroy
INFO 	Performing prerun...
INFO 	Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/b984a4/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO 	Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/b984a4/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO 	Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/b984a4/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO 	Running podman > create
INFO 	Sanity checks: 'podman'

PLAY [Create] ******************************************************************

TASK [get podman executable path] **********************************************
ok: [localhost]

TASK [save path to executable as fact] *****************************************
ok: [localhost]

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item="centos7 registry username: None specified")

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item=Dockerfile: None specified)

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item="Dockerfile: None specified; Image: docker.io/pycontribs/centos:7")

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item=centos7)

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item=docker.io/pycontribs/centos:7)

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item="centos7 command: None specified")

TASK [Remove possible pre-existing containers] *********************************
changed: [localhost]

TASK [Discover local podman networks] ******************************************
skipping: [localhost] => (item=centos7: None specified)

TASK [Create podman network dedicated to this scenario] ************************
skipping: [localhost]

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos7)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item=centos7)

PLAY RECAP *********************************************************************
localhost              	: ok=8	changed=3	unreachable=0	failed=0	skipped=5	rescued=0	ignored=0

INFO 	Running podman > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]

TASK [Copy something to test use of synchronize module] ************************
--- before
+++ after: /etc/hosts
@@ -0,0 +1,7 @@
+127.0.0.1  	localhost
+::1	localhost ip6-localhost ip6-loopback
+fe00::0    	ip6-localnet
+ff00::0    	ip6-mcastprefix
+ff02::1    	ip6-allnodes
+ff02::2    	ip6-allrouters
+172.17.0.2 	952dc91470f6

changed: [centos7]

TASK [Include vector-role] *****************************************************

TASK [vector-role : Get vector distrib] ****************************************
changed: [centos7]

TASK [vector-role : Install vector package] ************************************
changed: [centos7]

TASK [vector-role : Redefine vector config name] *******************************
--- before: /etc/default/vector (content)
+++ after: /etc/default/vector (content)
@@ -2,3 +2,4 @@
 # This file can theoretically contain a bunch of environment variables
 # for Vector.  See https://vector.dev/docs/setup/configuration/#environment-variables
 # for details.
+VECTOR_CONFIG=/etc/vector/config.yaml

changed: [centos7]

TASK [vector-role : Create vector config] **************************************
--- before
+++ after: /root/.ansible/tmp/ansible-local-2311p1enzhqs/tmpsb710pu9
@@ -0,0 +1,27 @@
+api:
+  address: 0.0.0.0:8686
+  enabled: true
+sinks:
+  to_clickhouse:
+	compression: gzip
+	database: vector_logs
+	endpoint: http://178.154.204.25:8123
+	inputs:
+	- parse_logs
+	table: logs_logs
+	type: clickhouse
+sources:
+  logs_logs:
+	format: syslog
+	interval: 1
+	type: demo_logs
+transforms:
+  parse_logs:
+	inputs:
+	- logs_logs
+	source: '. = parse_syslog!(string!(.message))
+
+  	.timestamp = to_string(.timestamp)
+
+  	.timestamp = slice!(.timestamp, start:0, end: -1)'
+	type: remap

changed: [centos7]

PLAY RECAP *********************************************************************
centos7                	: ok=6	changed=5	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0

INFO 	Running podman > verify
INFO 	Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [centos7] => {
	"changed": false,
	"msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos7                	: ok=1	changed=0	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0

INFO 	Verifier completed successfully.
INFO 	Running podman > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) deletion to complete (299 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '493880643105.3580', 'results_file': '/root/.ansible_async/493880643105.3580', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost              	: ok=2	changed=2	unreachable=0	failed=0	skipped=0	rescued=0	ignored=0

INFO 	Pruning extra files from scenario ephemeral directory
________________________________________________ summary ________________________________________________
  py37-ansible210: commands succeeded
  py37-ansible30: commands succeeded
  congratulations :)
[root@952dc91470f6 vector-role]#


```

</details>

9. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

[1.2.0](https://github.com/12sergey12/vector-role/releases/tag/1.2.0)

После выполнения у вас должно получится два сценария molecule и один tox.ini файл в репозитории. Не забудьте указать в ответе теги решений Tox и Molecule заданий. В качестве решения пришлите ссылку на  ваш репозиторий и скриншоты этапов выполнения задания. 

## Необязательная часть

1. Проделайте схожие манипуляции для создания роли LightHouse.
2. Создайте сценарий внутри любой из своих ролей, который умеет поднимать весь стек при помощи всех ролей.
3. Убедитесь в работоспособности своего стека. Создайте отдельный verify.yml, который будет проверять работоспособность интеграции всех инструментов между ними.
4. Выложите свои roles в репозитории.

В качестве решения пришлите ссылки и скриншоты этапов выполнения задания.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.
