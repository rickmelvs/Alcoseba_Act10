# Alcoseba_Act10

- name: Install Logstash
  apt:
    name: logstash
    state: present
    update_cache: yes

- name: Configure Logstash
  template:
    src: logstash.yml.j2
    dest: /etc/logstash/logstash.yml
  notify: restart logstash
