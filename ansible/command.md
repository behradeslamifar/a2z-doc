قبل از استفاده فایل playbook را تست کنید
```
ansible-playbook playbook.yml --check
```

استفاده از playbook
```
ansible-playbook -i host playbook.yml
ansible-playbook -i host playbook.yml --start-at-task='task_name'
ansible-playbook -i host playbook.yml --step
ansible-playbook -i host playbook.yml --extra-vars "pkg_name=postfix user=devops"
```

کمک بگیرید
```
ansible-doc <--list|-l>
ansible-doc <module_name>
```

استفاده از دستور ansibel
```
ansible local -m setup
ansible local -m setup --tree /tmp/facts
ansible local -m setup -a 'filter=*ipv4*'
ansible all --list-hosts
```

رمزنگاری فایل کلمات عبور
```
ansible-vault create secret.yml
ansible-vault edit secret.yml
ansible-vault rekey secret.yml
ansible-vault decrypt secret.yml

ansible-playbook playbook.yml <--ask-vault|--vault-password-file>
```