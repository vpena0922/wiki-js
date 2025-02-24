# Ansible

## Step 1 — Installing Ansible

To begin using Ansible as a means of managing your server infrastructure, you need to install the Ansible software on the machine that will serve as the Ansible control node.

From your control node, run the following command to include the official project’s PPA (personal package archive) in your system’s list of sources:

```bash
sudo apt-add-repository ppa:ansible/ansible
```

Press `ENTER` when prompted to accept the PPA addition.

Next, refresh your system’s package index so that it is aware of the packages available in the newly included PPA:

```bash
sudo apt update
```

Following this update, you can install the Ansible software with:

```bash
sudo apt install ansible
```

Your Ansible control node now has all of the software required to administer your hosts. Next, we will go over how to add your hosts to the control node’s inventory file so that it can control them.

## Step 2 — Setting Up the Inventory File

The *inventory file* contains information about the hosts you’ll manage with Ansible. You can include anywhere from one to several hundred servers in your inventory file, and hosts can be organized into groups and subgroups. The inventory file is also often used to set variables that will be valid only for specific hosts or groups, in order to be used within playbooks and templates. Some variables can also affect the way a playbook is run, like the `ansible_python_interpreter` variable that we’ll see in a moment.

To edit the contents of your default Ansible inventory, open the `/etc/ansible/hosts` file using your text editor of choice, on your Ansible control node:

```bash
sudo nano /etc/ansible/hosts
```

**Note**: Although Ansible typically creates a default inventory file at `etc/ansible/hosts`, you are free to create inventory files in any location that better suits your needs. In this case, you’ll need to provide the path to your custom inventory file with the `-i` parameter when running Ansible commands and playbooks. Using per-project inventory files is a good practice to minimize the risk of running a playbook on the wrong group of servers.

The default inventory file provided by the Ansible installation contains a number of examples that you can use as references for setting up your inventory. The following example defines a group named `[servers]` with three different servers in it, each identified by a custom alias: **server1**, **server2**, and **server3**. Be sure to replace the highlighted IPs with the IP addresses of your Ansible hosts.

/etc/ansible/hosts

```
[servers]
server1 ansible_host=203.0.113.111
server2 ansible_host=203.0.113.112
server3 ansible_host=203.0.113.113

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

The `all:vars` subgroup sets the `ansible_python_interpreter` host parameter that will be valid for all hosts included in this inventory. This parameter makes sure the remote server uses the `/usr/bin/python3` Python 3 executable instead of `/usr/bin/python` (Python 2.7), which is not present on recent Ubuntu versions.

When you’re finished, save and close the file by pressing `CTRL+X` then `Y` and `ENTER` to confirm your changes.

Whenever you want to check your inventory, you can run:

```bash
ansible-inventory --list -y
```

You’ll see output similar to this, but containing your own server infrastructure as defined in your inventory file:

```
Output
all:
  children:
    servers:
      hosts:
        server1:
          ansible_host:203.0.113.111
          ansible_python_interpreter: /usr/bin/python3
        server2:
          ansible_host:203.0.113.112
          ansible_python_interpreter: /usr/bin/python3
        server3:
          ansible_host:203.0.113.113
          ansible_python_interpreter: /usr/bin/python3
    ungrouped: {}

```

Now that you’ve configured your inventory file, you have everything you need to test the connection to your Ansible hosts.

## Step 3 — Testing Connection

After setting up the inventory file to include your servers, it’s time to check if Ansible is able to connect to these servers and run commands via SSH.

For this guide, we’ll be using the Ubuntu **root** account because that’s typically the only account available by default on newly created servers. If your Ansible hosts already have a regular sudo user created, you are encouraged to use that account instead.

You can use the `-u` argument to specify the remote system user. When not provided, Ansible will try to connect as your current system user on the control node.

From your local machine or Ansible control node, run:

```bash
ansible all -m ping -uroot
```

This command will use Ansible’s built-in [`ping` module](https://docs.ansible.com/ansible/latest/modules/ping_module.html) to run a connectivity test on all nodes from your default inventory, connecting as **root**. The `ping` module will test:

- if hosts are accessible;
- if you have valid SSH credentials;
- if hosts are able to run Ansible modules using Python.

You should get output similar to this:

```
Output
server1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
server2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
server3 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

If this is the first time you’re connecting to these servers via SSH, you’ll be asked to confirm the authenticity of the hosts you’re connecting to via Ansible. When prompted, type `yes` and then hit `ENTER` to confirm.

Once you get a `"pong"` reply back from a host, it means you’re ready to run Ansible commands and playbooks on that server.

**Note**: If you are unable to get a successful response back from your servers, check our [Ansible Cheat Sheet Guide](https://www.digitalocean.com/community/tutorials/how-to-use-ansible-cheat-sheet-guide) for more information on how to run Ansible commands with different connection options.

[**apt_update.yml**](Ansible%20bbcd979dffba45928a2624efa7d86431/apt_update%20yml%207d2c54001fe34a10b927983053ea9fe4.md)

[**new_ubuntu_server.yml**](Ansible%20bbcd979dffba45928a2624efa7d86431/new_ubuntu_server%20yml%209bb25304ab8e46f89d0b2eb18e87eacf.md)

[**windows_updates.yml**](Ansible%20bbcd979dffba45928a2624efa7d86431/windows_updates%20yml%20f5bcb352358b4f4f871dc5f977876c9f.md)