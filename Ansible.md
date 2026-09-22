In Amazon Linux 2023, packages are managed natively via dnf (which replaces yum), and ansible-core is available directly in the default repositories.

Solution 1: Install using the default Package Manager (Recommended)You can install Ansible natively using dnf without needing any extra repositories:
==========
sudo dnf install ansible-core -y

Solution 2: Install via Python pip (For the full Ansible package)If you need the comprehensive ansible community package (which includes additional collections and plugins) rather than just the minimal ansible-core:bash# Ensure pip is installed
==========

sudo dnf install python3-pip -y

# Install Ansible for your user
pip3 install ansible --user

(Note: If you install via pip --user, ensure your execution path includes ~/.local/bin by running export PATH=$PATH:$HOME/.local/bin)


verify ansible version by running below command

```bash
ansible --version
```

```bash
which ansible
```

**Create a common user for all ansible activities**

Creating a user, Here am using "ansibleusr" user and set password.

```bash
useradd ansibleusr
```
```bash
passwd ansibleusr
```

**Provide Sudeoer permissions**
```bash
visudo
```

Add below entry to sudeor file.

```bash
ansibleusr ALL=(ALL) NOPASSWD: ALL
```

```bash
su - ansibleusr
```

**Enable SSH Connection across the instances.**

```bash
vim /etc/ssh/sshd_config
```
```bash
Set "PasswordAuthentication yes"
```

```bash
service sshd restart
```

**Connect to all the nodes and Create common user same as Above.**

```bash
useradd ansibleusr
passwd ansibleusr
visudo
ansibleusr ALL=(ALL) NOPASSWD: ALL
su - ansibleusr
vim /etc/ssh/sshd_config
Set "PasswordAuthentication yes"
service sshd restart
```

**Configure password-less authentication.**

*Perform this on Control Node*

Create an SSH Public key and Private Key in Server and Copy Public Key to across the instances. Make sure you are doing this as ansibleusr, and perform this from Control Node.

```bash
ssh-keygen -t rsa
```

```bash
ssh-copy-id ansibleuser@Managed-node-1/2/3
```

Now test the Password-less authentication. It should not prompt any password.

```bash
ssh Node-ip
```


***When we install ansible, we will get 3 IMP files under /etc/ansible***
1. ansible-config
2. hosts
3. roles

In "ansible-config" file, hosts file entry is commented default.

Create a group in hosts file (i.e; webservers / web-group / dbservers)


**Commands to verify hosts**

To list all the hosts ansible is managing run below command.

```bash
ansible all --list-hosts
```

You can also ping all/group of nodes.

```bash
ansible all -m ping -v
```

If you want to list the nodes from a specific group, use below command

```bash
ansible mygroup --list-hosts
```
