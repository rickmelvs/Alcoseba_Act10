# Alcoseba_Act10
handlers for all roles

- name: restart elasticsearch
  service:
    name: elasticsearch
    state: restarted

- name: restart kibana
  service:
    name: kibana
    state: restarted

- name: restart logstash
  service:
    name: logstash
    state: restarted

main.yml for each role tasks

- name: Ubuntu
  import_tasks: ubuntu.yml
  when: ansible_os_family == "Debian"

- name: CentOS
  import_tasks: centos.yml
  when: ansible_os_family == "RedHat"
