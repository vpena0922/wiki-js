# new_ubuntu_server.yml

```yaml
---
- name: Update and configure Ubuntu
  hosts: "newservers"
  become: true
  become_method: sudo

  vars:
    new_root_password: "{{ lookup('env', 'NEW_ROOT_PASSWORD') }}"  # Get the password from an environment variable
  
  tasks:

    - name: set timezone
    shell: timedatectl set-timezone America/New_York
    
    - name: Update apt package cache
      apt:
        update_cache: yes
        upgrade: dist
    
    - name: Install updates
      apt:
        upgrade: dist

    - name: Enable root login by modifying sshd_config
      lineinfile:
        path: /etc/ssh/sshd_config
        regexp: '^#?PermitRootLogin'
        line: 'PermitRootLogin yes'
        state: present
  
    - name: Set root password
      user:
        name: root
        password: "{{ new_root_password | password_hash('sha512') }}"
        update_password: always

    - name: Add public key to authorized_keys
      authorized_key:
        user: root
        key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDU/3bQXRV5EbylSBBdqicOvUchNn/Wgsw7hQClRrfOlqT3TES8AsRKWugiPF+aP5av0JjEgN1UcHonjbtmto2JFCu4WC0Dab63ChDPkVHGO2Uwh1j3cxGx5INDeIsS1nDjzZ0QoVyfsJ/LmSdk1yHYicpeArYTn+qK2Nu8fG0QGYJlnM/skiDObN+X8I6r1kCrx2abff4dgoVmV9UnQyN8P7kJgRfrPcQuFQs3lm0rAaoprwPT18qMG8CMROrGx5iHBrVYletam5fSqQQUUE+ib86i1MhfxUWi/iSqIdnl0h2cRm0Xqggy9itSIINXjeLs1YsNkbXxN4w0bNVr7W3X victor.pena@Victors-MacBook-Pro.local"
        state: present

    - name: Reboot the machine
      ansible.builtin.reboot:
```