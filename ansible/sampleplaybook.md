```
---# This is another comment
- hosts: all
  user: devops
  sudo: yes
  connection: <ssh|local>
  gather_facts: no
  vars:
    zabbix_server: 127.0.0.1
    author: Behrad Eslamifar
  vars_files:
    - vars.yml
  vars_promt:
    - name: domain
      prompt: Ender domain name
      default: linuxmotto.ir
      private: no
  tasks:
  - include: path/to/taskfile.yml
  - name: This is first task
    module_name:
    ignore_errors: yes
  - name: This is yum task
    action: yum name=tmux state=installed
    async: 300    # Parallel running wait 300 second
    pol: 3        # pol every 3 second to check status
    notify: Restart service
  - name: verify service is running
    command: systemctl status postfix
    register: stats
  - debug: var=stats
  - name: Add list of users with loop
    user: name={{ item }} state=present
    with_items:
      - behrad
      - narges
  - name: run command without stats back to ansible
    raw: /usr/bin/date > /var/log/ansible_run.log
  - name: Install nginx on ubuntu
    apt: name=nginx state=present
    when: ansible_os_family == "Ubuntu"
  - name: Is sevice running
    commaand: systemctl status postfix
    register: result
    until: result.stdout.find("active (running)") != -1
    retries: 5
    delay: 5
  - debug: var=result

  - handlers: 
      - name: Restart service
        action: service name=postfix state=restarted
```

```
$ cat vars.yml
zabbix_server: 127.0.0.1
```

استفاده از template
```
---# This playbook to test template
- hosts: all
  user: devops
  sudo: yes
  connection: <ssh|local>
  gather_facts: no
  vars:
    my_server: 127.0.0.1
  tasks:
    - name: use template for config
      template: src=file.conf.j2 dest=/etc/file.conf owner=root group=root mode=644
```

```
# file.conf.j2 file for test template in ansible
Server={{ my_server }}
```