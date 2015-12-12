---
layout: post
title: Ansible basics
date: 2015-11-17 14:25:00 +0800
author: Andy Jiang
---

Ansible - command from command line and playbook

#Execute simple commands on remote node with Ansible from command line
{% highlight bash %}
$ ansible target -m ping -u ubuntu -k
SSH password:
10.10.10.1 | success >> {
    "changed": false,
    "ping": "pong"
}
=> target: the host/ip/group in /etc/ansible/hosts file that is being configured
=> -m: the module to call
=> -u: the username to use for ssh connection with the target machine
=> -k: same as "--ask-pass", prompt for asking ssh password for connection

$ ansible webservers -m yum -a "name=acme state=present"
=> -a: the argument of the module
{% endhighlight %}

#Execute complex commands on remote node with Ansible from playbook
Ansible playbooks include multiple tasks as yaml-formatted file and run together

- Ansible playbook example and saved as: playbook.yml
{% highlight yaml %}
---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  vars_files:
    - /vars/external_vars.yml
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum: name=httpd state=latest
  - name: write the apache config file
    template: src=/srv/httpd.j2 dest=/etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running (and enable it at boot)
    service: name=httpd state=started enabled=yes
  handlers:
    - name: restart apache
      service: name=httpd state=restarted
{% endhighlight %}

- Run Ansible playbook
{% highlight bash %}
$ ansible-playbook playbook.yml
{% endhighlight %}

- Include files for reuse
{% highlight yaml %}
---
# possibly saved as tasks/foo.yml
- name: placeholder foo
  command: /bin/foo
- name: placeholder bar
  command: /bin/bar

# the task that invoke/include the above foo.yml
  tasks:
    - include: tasks/foo.yml
      vars:
          var1: value1
          var2:
              - value2_1
              - value2_2
{% endhighlight %}

- Pass extra arguments from command line
{% highlight bash %}
$ ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"
{% endhighlight %}
