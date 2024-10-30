# Alcoseba_Act10

- name: Install Logstash
  yum:
    name: logstash
    state: present

- name: Configure Logstash
  template:
    src: logstash.yml.j2
    dest: /etc/logstash/logstash.yml
  notify: restart logstash
