# ---
  - name: Play for Install Nginx on WebServers
    hosts: localhost
    gather_facts: yes
    vars:
      custom_heading: "Welcome To DevOps Training B15 By Sreeharsha Veerapalli - Testing AWX"
      todays_date: "{{ ansible_facts['date_time']['date'] }}"
      host_name: "{{ ansible_facts['hostname'] }}"
      fqdn_name: "{{ ansible_facts['fqdn'] }}"
      ip_address: "{{ ansible_facts['eth0']['ipv4']['address'] }}"
    tasks:
       - name: Run Apt Update
         shell: apt update && apt install -y python-apt

       - name: Install Nginx Server
         apt: >
           name=nginx
           state=present
       - name: Copy the files to index destination folder.
         template: >
            src=/tmp/ansibletemplatetesting/index.j2
            dest=/var/www/html/index.nginx-debian.html
            owner=root
            group=root
            mode=0644
       - name: Copy the style files to destination folder.
         copy: >
            src=/tmp/ansibletemplatetesting/style.css
            dest=/var/www/html/style.css
            owner=root
            group=root
            mode=0644
       - name: Copy the javascript files to destination folder.
         copy: >
            src=/tmp/ansibletemplatetesting/scorekeeper.js
            dest=/var/www/html/scorekeeper.js
            owner=root
            group=root
            mode=0644
       - name: restart nginx
         command: service nginx restart
