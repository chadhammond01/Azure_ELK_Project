## Automated ELK Stack Deployment

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The files in this repository were used to configure the network in Azure as depicted below.

![Network Diagram](Diagrams/Azure%20Network%20Diagram.PNG)

_Alternatively, select portions of the **[webservers.yml](Ansible)** file may be used to install only certain pieces of it, such as Filebeat._

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.  
Load balancing ensures that the application will be highly available by sharing incoming traffic amoung the vulnerable web servers. In addition, inbound access to the network will be restricted through access controls so only authorized users will be able to connect.

Integrating an ELK server allows users to easily monitor for changes to the file systems of the VMs on the network and as well as watch system metrics, such as CPU usage; attempted SSH logins; sudo escalation failures; etc.

The configuration details of each machine may be found below.

| Name      | Function   | IP Address | Operating System |
|-----------|------------|------------|------------------|
| Jump Box  | Gateway    | 10.0.0.5   | Linux            |
| Web-1     | Web Server | 10.0.0.6   | Linux            |
| Web-2     | Web Server | 10.0.0.7   | Linux            |
| Web-3     | Web Server | 10.1.0.6   | Linux            |
| Web-4     | Web Server | 10.1.0.7   | Linux            |
| ELK Stack | Monitoring | 10.1.0.5   | Linux            |

In addition to the above, Azure has provisioned a **load balancer** in front of all machines except for the jump box.  
The load balancer's targets are organized into the following availability zones:
- **Availability Zone 1**: Web-1 + Web-2
- **Availability Zone 2**: Web-3 + Web-4

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **Jump Box** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 67.164.160.6

Machines _within_ the network can only be accessed internally.  

A summary of the access policies in place can be found in the Azure screenshot below.

![](Images/azurensgrules.PNG)

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

```yaml
---
# elkserver.yml
- name: Configure Elk VM with Docker
  hosts: elkservers
  remote_user: elk
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

    - name: Install Docker python module
      pip:
        name: docker
        state: present

    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        # Please list the ports ELK runs on
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044
```

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![ELK Container](Images/elkdocker.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

| Name      | IP Address | 
|-----------|------------|
| Web-1     | 10.0.0.6   | 
| Web-2     | 10.0.0.7   | 
| Web-3     | 10.1.0.6   |
| Web-4     | 10.1.0.7   |

The following Beats on these machines:
- Filebeat
- Metricbeat

These beats allow the collection of the following information from each machine:
- **Filebeat**: Filebeat detects changes to the filesystem. Specifically, used to collect Apache logs.
- **Metricbeat**: Metricbeat detects changes in system metrics. Detects SSH login attempts, failed `sudo` escalations, and CPU/RAM statistics.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
