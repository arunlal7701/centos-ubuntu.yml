# centos-ubuntu.yml
Single playbook to install apache on both ubuntu and centos
```
---
- name: "Installing Httpd And Hosting A Site"
  hosts: all
  become: true
  tasks:
    
    - name: "centos - Installing Apache Webserver"
      when: ansible_os_family == "RedHat"
      yum:
        name: httpd
        state: present

    - name: "centos - Copying Website Contents"
      when: ansible_os_family == "RedHat"
      copy:
        src: ./website/
        dest: /var/www/html/
        owner: apache
        group: apache
          
        
    - name: "centos - Restarting apache service and enabling"
      when: ansible_os_family == "RedHat"
      service:
        name: httpd
        state: restarted
        enabled: true
          
    - name: "ubuntu - Installing Apache Webserver"
      when: ansible_os_family == "Debian"
      apt:
        name: apache2
        state: present

    - name: "ubuntu - Copying Website Contents"
      when: ansible_os_family == "Debian"
      copy:
        src: ./website/
        dest: /var/www/html/

          
        
    - name: "ubuntu - Restarting apache service and enabling"
      when: ansible_os_family == "Debian"
      service:
        name: apache2
        state: restarted
        enabled: true
        
