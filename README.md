# hello-world
---
- hosts: 'apache-httpd:&apt'
  sudo: true
  tasks:

    - name: Install Apache web server
      apt: 'pkg={{item}} state=present'
      with_items:
        - apache2
        - apache2-threaded-dev

- hosts: apache-httpd
  sudo: true
  tasks:

    - include: ssl.yaml
      when: 'apache_https["hostname"] != "none"'

  handlers:
    - name: stop Apache for reconfiguration
      service: 'name=apache2 state=stopped'

- hosts: apache-httpd
  sudo: true
  tasks:
    - name: ensure Apache is running
      service: 'name=apache2 state=started'
