---
- hosts: all
  become: true
  pre_tasks:

  - name: update repository index / install Updates (CentOS)
    tags: always
    dnf:
      update_cache: yes
    changed_when: false
    when: ansible_distribution == "CentOS"

  - name: update repository index / install Updates (Ubuntu)
    tags: always
    apt:
      update_cache: yes
    changed_when: false
    when: ansible_distribution == "Ubuntu"
    
- hosts: CentOS
  become: true
  roles:
   - CentOS

- hosts: Ubuntu1
  become: true
  roles:
   - Ubuntu1
   
- hosts: Ubuntu2
  become: true
  roles:
   - Ubuntu2
