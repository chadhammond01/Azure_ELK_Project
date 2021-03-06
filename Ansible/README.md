## Webserver Playbook Roles:

As written, the **[Webserver playbook](/Ansible/webserver.yml)** will install and enable `dwva`, `filebeat`, and `metricbeat` roles on the Webserver VM's

The playbook is duplicated below. To skip installation of a specific role, simply comment it out.

```yaml
---
# webserver.yml
- become: true
  hosts: webservers
  name : Installing DVWA with Docker
  roles:
    - dvwa
    - filebeat
    - metricbeat
    - heartbeat
    - packetbeat
```

#### Directory Structure for Roles:

Each role directory must be configured with its own contents, and the corresponding directories contain all the files needed.

```
/etc/
  ansible/
    hosts
    ansible.cfg
    webserver.yml
    elkserver.yml
    roles/
      dvwa/
        tasks/
          main.yml
      filebeat/
        tasks/
          main.yml
        files/
          filebeat-config.yml
      metricbeat/
        tasks/
          main.yml
        files/
          metricbeat-config.yml
      heartbeat/
        tasks/
          main.yml
        files/
          heartbeat-config.yml
      packetbeat/
        tasks/
          main.yml
        files/
          packetbeat-config.yml
```
---
### Using the Playbook
In order to use the playbook, you will need to have an Ansible Control Node already configured. 

SSH into the control node and follow the steps below:
- Copy the playbooks to the Ansible Control Node
- Run each playbook on the appropriate targets

The easiest way to copy the playbooks is to use Git. This will copy the playbooks and required files to the correct place:

```bash
$ cd /etc/ansible
# Clone Repository + IaC Files
$ git clone https://github.com/chadhammond01/Azure_ELK_Project.git
# Move Playbooks, roles, and hosts files Into `/etc/ansible`
$ cp Azure_ELK_Project/Ansible/* -R .
```

Next, you must edit the `hosts` file to specify which VMs to run each playbook on. For example:

```
 [webservers]
10.0.0.6 ansible_python_interpreter=/usr/bin/python3
10.0.0.7 ansible_python_interpreter=/usr/bin/python3
```

After this, the commands below run the playbook:

 ```bash
 $ cd /etc/ansible
 $ ansible-playbook webserver.yml
 ```
 
To verify success, run `curl http://10.0.0.6:80/setup.php` If the installation succeeded, this command should print HTML to the console.
