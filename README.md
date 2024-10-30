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
