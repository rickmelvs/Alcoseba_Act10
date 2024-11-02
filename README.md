- name: Add Elastic APT key
      apt_key:
        url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
        state: present
      when: ansible_distribution == "Ubuntu"

    - name: Add Elastic repository
      apt_repository:
        repo: "deb https://artifacts.elastic.co/packages/8.x/apt stable main"
        state: present
      when: ansible_distribution == "Ubuntu"
    
    - name: Update APT package cache
      apt:
        update_cache: yes
      when: ansible_distribution == "Ubuntu"

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
        enabled: yes
      become: yes
      when: ansible_distribution == "Ubuntu"
