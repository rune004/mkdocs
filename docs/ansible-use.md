# Use a Ansible-playbook

![ansible](img/ansible.png)

### Find the right folder
This will be the "root" folder for Ansible.
```
cd /etc/ansible
```

### Use SSH without having to type in the password
First "Read" your SSH key.
```
eval `ssh-agent`
```
and then "Load" your SSH key with your password.
```
ssh-add
```

### Use your playbooks 
First make sure you are in the same folder as the playbook you want to use.

Then write:
```
ansible-playbook Win-Test-Env.yaml
```

