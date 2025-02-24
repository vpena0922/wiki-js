# apt_update.yml

```yaml
- hosts: "ubuntu"
  tasks:
    - name: apt
      apt:
        update_cache: yes
        upgrade: 'dist'
```