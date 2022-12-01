# Setup/Install Ansible

*This is how I automated the deployment of a test environment, with 2 Windows Servers VMs as DC controllers and 3 Windows 10 VMs as client. All 3 clients have been sign into the Windows AD, and so have both the Windows Servers.*

![ansible](img/ansible-ubuntu.png)

*This was made for and deployed onto a Proxmox Server with ansible running on a Ubuntu VM from my Windows laptop as controller.*

## Installation on Ubuntu VM

--------------------

### Install sudo
This will be needed later for sudo nano.
```
apt-get install sudo 
```
---------------

### Install curl
This will be needed later to easier download the playbooks from github and install TailScale VPN
```
sudo apt install curl
```
Or
```
sudo apt-get install curl
```

----------------

### Install pip
This will be needed later to download/install ansible and it's modules and also update blowfish.
```
sudo apt-get install python3 python3-pip -y
```

----------------

### Install Ansible
Install Ansible and make sure the everything is as it should.
```
sudo pip3 install ansible
```
Check that Ansible was installed with the command below.
```
ansible --version
```

----------------

## Prep Proxmox Server


### Install Proxmoxer
This will be needed to deploy VMs via Ansible on the Proxmox server.
This has to be done on the Proxmox server ifself.
```
pip install proxmoxer
```
```
pip install requests
```
```
pip install paramiko
```
```
pip install openssh_wrapper
```

-------------------

### Update Blowfish
Removes a warning when using ssh of Blowfish being deprecated. 
(Do this to both Proxmox Server and Ubuntu VM)
```
pip install blowfish
```


--------------------

## Configuration of Ansible controller

![ansible-ssh](img/Ansible-ssh.png)

After installing Ansible, the file /etc/hosts file is created automatically. 
In this file, we are required to add our managed nodes.

In the file, add your managed nodes as below.

```
[Node1]
192.168.100.118
[Node2]
192.168.100.119
```

### Modify Ansible.cfg 
Find and change the following settings in 
```
sudo nano /etc/ansible/ansible.cfg
```
```
inventory = /etc/ansible/hosts

forks = 5
sudo_user = root

remote_user = root

become=True
become_method=sudo
become_user=root
become_ask_pass=False
```

### Push SSH key to nodes
This is done so that Ansible can use the SSH key to sign in as a user
```
ssh-copy-id -i ~/.ssh/id_rsa.pub your_username@192.168.100.118
```


### Allow root login
(login to all nodes and do the following)

To enable SSH login for a root user on Debian Linux system you need to first configure SSH server. 
```
sudo nano /etc/ssh/sshd_config
```

Change the following line:
```
FROM:
PermitRootLogin without-password
TO:
PermitRootLogin yes
```

Once you made the above change restart your SSH server:
```
# /etc/init.d/ssh restart
[ ok ] Restarting ssh (via systemctl): ssh.service.
```



## References 

1. [Install and use ansible on debian linux](https://computingforgeeks.com/install-and-use-ansible-on-debian-linux/)
2. [Blowfish](https://pypi.org/project/blowfish/)
3. [Proxmoxer](https://pypi.org/project/proxmoxer/)
4. [Install pip on debian](https://linuxhint.com/install-pip-on-debian-11/)
5. [Install curl on debian](https://www.cyberciti.biz/faq/howto-install-curl-command-on-debian-linux-using-apt-get/)
6. [Enable sudo user account debian](https://milq.github.io/enable-sudo-user-account-debian/)
