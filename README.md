var-merger
=========

Роль объеденяет указанные массивы, точечно реализуя конфигурацию ANSIBLE_HASH_BEHAVIOUR = merge.
При указании merge_group_vars = True объеденяются переменные всех групп к которым принадлежит хост. 

На текущий момент реализовано объеденение переменных только следующих категорий:
playbook group_vars/all
playbook group_vars/*

Переменные
------------

 * `to_merge` - массив переменных для объеденения
 * `merge_group_vars` - объеденять или нет групповые переменные (default: False)

Пример объеденения
------------
playbook/group_vars/all
```YML
smb_config: 
  share:
    aoo: 1
    boo: 2
```

playbook/group_vars/A.yml
```YML
smb_config:
  share:
    boo: 5
    coo: 3
```

playbook/group_vars/B.yml
```YML
smb_config: 
  share:
    doo: 4
```

При merge_group_vars = True, получим:
```YML
smb_config:
  share:
    aoo: 1
    boo: 5
    coo: 3
    doo: 4
```

При merge_group_vars = False, получим:
```YML
smb_config: 
  share:
    aoo: 1
    boo: 2
    doo: 4
```


Приоритет переменных 2.9 Ansible (для будущей реализации)
------------

```YML
command line values (eg “-u user”)
role defaults [1]
inventory file or script group vars [2]
inventory group_vars/all [3]
playbook group_vars/all [3]
inventory group_vars/* [3]
playbook group_vars/* [3]
inventory file or script host vars [2]
inventory host_vars/* [3]
playbook host_vars/* [3]
host facts / cached set_facts [4]
play vars
play vars_prompt
play vars_files
role vars (defined in role/vars/main.yml)
block vars (only for tasks in block)
task vars (only for the task)
include_vars
set_facts / registered vars
role (and include_role) params
include params
extra vars (always win precedence)
```