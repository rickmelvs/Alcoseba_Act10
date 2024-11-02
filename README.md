# Alcoseba_Act10

---
- name: Install Java JDK
  yum:
    name: java-11-openjdk
    state: present
  when: ansible_distribution == "CentOS"

- name: Add EPEL Repository
  yum:
    name: epel-release
    state: latest
  when: ansible_distribution == "CentOS"

- name: Add Elasticsearch Repository
  yum_repository:
    name: elasticsearch
    description: Elasticsearch Repository
    baseurl: https://artifacts.elastic.co/packages/7.x/yum
    gpgcheck: yes
    gpgkey: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    enabled: yes
  when: ansible_distribution == "CentOS"

- name: Install Elasticsearch
  yum:
    name: elasticsearch
    state: present
  when: ansible_distribution == "CentOS"

- name: Configure Elasticsearch
  template:
    src: elasticsearch.yml.j2
    dest: /etc/elasticsearch/elasticsearch.yml
  when: ansible_distribution == "CentOS"

- name: Start and Enable Elasticsearch Service
  service:
    name: elasticsearch
    state: restarted
    enabled: yes
  when: ansible_distribution == "CentOS"

- name: Open Port 9200 in Firewall
  command: firewall-cmd --zone=public --add-port=9200/tcp --permanent
  register: firewall_result
  ignore_errors: true

- name: Reload Firewall
  command: firewall-cmd --reload
  when: firewall_result.changed
