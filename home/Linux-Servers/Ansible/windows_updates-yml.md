# windows_updates.yml

```yaml
---
- name: Update Windows systems
  hosts: windows
  tasks:
    
    - name: Install all updates and reboot as many times as needed
      ansible.windows.win_updates:
        category_names: '*'
        reboot: true
```