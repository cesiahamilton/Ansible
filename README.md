# Ansible

Filebeat playbook.yml 

---
     - name: Installing and launch filebeat
       hosts: webservers
       become: yes
       tasks:


       - name: download filebeat .deb file
         command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebe$
       - name: Install filebeat deb
         command: dpkg -i filebeat-7.6.2-amd64.deb

       - name: drop in filebeat.yml
         copy:
           src: /etc/ansible/files/filebeat-config.yml
           dest: /etc/filebeat/filebeat.yml

       - name: enable and configure system module
         command: filebeat modules enable system

       - name: setup filebeat
         command: filebeat setup --dashboards

       - name: start filebeat service
         command: service filebeat start

       - name: enable service filebeat on boot
         systemd:
           name: filebeat
           enabled: yes
           
Filbeat.config.yml
![filebeat config host name](https://user-images.githubusercontent.com/77912577/116796717-d20b8800-aaac-11eb-8981-bf298bb8c7e5.jpg)
![filebeat config kibana name](https://user-images.githubusercontent.com/77912577/116796718-d899ff80-aaac-11eb-8077-0a572490111e.jpg)

Metricbeat-playbook.yml
root@651f32ade919:/etc/ansible/files# ls
filebeat-config.yml  filebeat-playbook.yml  metricbeat-config.yml  metricbeat-playbook.yml
root@651f32ade919:/etc/ansible/files# cat metricbeat-playbook.yml
---
-  name: Installing and launching Metricbeat
   hosts: webservers
   become: yes
   tasks:

       - name: Download Metricbeat .deb file
         command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb

       - name: install Metricbeat .deb file
         command: dpkg -i metricbeat-7.6.1-amd64.deb

       - name: drop in Metricbeat.yml
         copy:
           src: /etc/ansible/files/metricbeat-config.yml
           dest: /etc/metricbeat/metricbeat.yml

       - name: enable and configure metric modules
         command: metricbeat modules enable docker

       - name: setup Metricbeat
         command: metricbeat setup

       - name: start Metricbeat
         command: service metricbeat start
         
         
Metricbeat-config.yml
![metricbeat config host name](https://user-images.githubusercontent.com/77912577/116796808-6e358f00-aaad-11eb-9480-61807d38c877.jpg)
![metricbeat config kibana name](https://user-images.githubusercontent.com/77912577/116796810-7392d980-aaad-11eb-8f01-a659e55b733d.jpg)

Pentest.yml
root@651f32ade919:/etc/ansible# ls
ansible.cfg  files  hosts  install-elk.yml  pentest.yml  roles
root@651f32ade919:/etc/ansible# cat pentest.yml
---
  - name: Config Web VM with Docker
    hosts: webservers
    become: true
    tasks:

    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    - name: Install Python Docker module
      pip:
        name: docker
        state: present

    - name: download and launch a docker web container
      docker_container:
        name: dvwa
        image: cyberxsecurity/dvwa
        state: started
        restart_policy: always
        published_ports: 80:80

    - name: Enable docker service
      systemd:
        name: docker
        enabled: yes



