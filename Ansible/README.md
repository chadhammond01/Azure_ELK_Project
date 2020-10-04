## Webserver Playbook Roles:

As written, the **[Webserver playbook](/Ansible/webserver.yml)** will install and enable `dwva`, `filebeat`, and `metricbeat` on the Webserver VM's

Each role directory must be configured with its own contents, and the corresponding directories contain all the files needed.

#### Directory Structure:

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
```
