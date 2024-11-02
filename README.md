- hosts: all
  become: true
  pre_tasks:

  - name: Update repository index / install updates on CentOS
    tags: always
    yum:
      update_cache: yes
    changed_when: false
    when: ansible_distribution == "CentOS"

  - name: Update repository index / install updates on Ubuntu
    tags: always
    apt:
      update_cache: yes
    changed_when: false
    when: ansible_distribution == "Ubuntu"
    
- hosts: centos_servers
  become: true
  roles:
   - centos_role

- hosts: ubuntu_server1
  become: true
  roles:
   - ubuntu_role1
   
- hosts: ubuntu_server2
  become: true
  roles:
   - ubuntu_role2
