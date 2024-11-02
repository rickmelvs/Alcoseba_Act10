# Alcoseba_Act10

---
- name: Add Elasticsearch GPG Key
  apt_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    state: present
  when: ansible_distribution == "Ubuntu"

- name: Add Kibana APT Repository
  apt_repository:
    repo: "deb https://artifacts.elastic.co/packages/7.x/apt stable main"
    state: present
  when: ansible_distribution == "Ubuntu"

- name: Install Kibana
  apt:
    name: "kibana=7.17.25"
    state: present
  when: ansible_distribution == "Ubuntu"

- name: Create Kibana Systemd Override Directory
  file:
    path: /etc/systemd/system/kibana.service.d
    state: directory
    mode: '0755'
    owner: root
    group: root
  when: ansible_distribution == "Ubuntu"

- name: Check Kibana Override Directory
  stat:
    path: /etc/systemd/system/kibana.service.d
  register: kibana_override_dir

- name: Debug Directory Status
  debug:
    msg: "Directory exists: {{ kibana_override_dir.stat.exists }}"

- name: Create Kibana Override Config File
  file:
    path: /etc/systemd/system/kibana.service.d/override.conf
    state: touch
    owner: root
    group: root
    mode: '0644'
  when: ansible_distribution == "Ubuntu"

- name: Add OpenSSL Legacy Provider to Kibana Config
  blockinfile:
    path: /etc/systemd/system/kibana.service.d/override.conf
    block: |
      [Service]
      Environment=NODE_OPTIONS=--openssl-legacy-provider
    owner: root
    group: root
    mode: '0644'
  when: ansible_distribution == "Ubuntu"

- name: Apply Kibana Configuration
  template:
    src: kibana.yml.j2
    dest: /etc/kibana/kibana.yml
  when: ansible_distribution == "Ubuntu"

- name: Reload Systemd
  command: systemctl daemon-reload
  when: ansible_distribution == "Ubuntu"

- name: Start and Enable Kibana Service
  service:
    name: kibana
    state: restarted
  become: yes
  when: ansible_distribution == "Ubuntu"
