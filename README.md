# Alcoseba_Act10

---
- name: Install Logstash Package
  apt:
    name: logstash
    state: latest
  when: ansible_distribution == "Ubuntu"

- name: Set Permissions for Logstash Data Directory
  file:
    path: /usr/share/logstash/data
    state: directory
    mode: '0755'
    owner: logstash
    group: logstash
  when: ansible_distribution == "Ubuntu"

- name: Apply Logstash Configuration
  template:
    src: logstash.conf.j2
    dest: /etc/logstash/conf.d/logstash.conf
  when: ansible_distribution == "Ubuntu"

- name: Open Port 9200 in UFW
  ufw:
    rule: allow
    port: "9200"
    proto: tcp
  when: ansible_distribution == "Ubuntu"

- name: Start and Enable Logstash Service
  service:
    name: logstash
    state: started
    enabled: yes  # Ensures service starts on boot
  become: yes
  when: ansible_distribution == "Ubuntu"
