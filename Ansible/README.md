## Roles:
A **role** is a playbook dedicated to a single task, such as installing Filebeat. A role might be responsible for performing the following tasks:
- Downloading and unarching a file.
- Installing software.
- Copying configuration files into place.
- Starting services.

For example, the Webserver playbook will run the `dwva`, `filebeat`, and `metricbeat` tasks on the Webserver VM

Each role directory must be configured with its own contents, and the corresponding directories contain all the files needed for the project.

### Directory structure:

```
/etc/
  ansible/
    hosts
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
