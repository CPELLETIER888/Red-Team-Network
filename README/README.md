### Automated ELK Stack Deployment ###

The files in this repository were used to configure the network depicted below.

(https://drive.google.com/file/d/1WYy9mG1C7KwO4gwtgJrV7qjG8E69cK5i/view?usp=sharing)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  - install_elk.yml

---
- name: Configure Elk VM with Docker
 hosts: elkserver
 remote_user: sysadmin
 become: true
 tasks:
# Use apt module
   - name: Install docker.io
     apt:
       update_cache: yes
       name: docker.io
       state: present

# Use apt module
   - name: Install pip3
     apt:
       force_apt_get: yes
       name: python3-pip
       state: present

# Use pip module
   - name: Install Docker python module
     pip:
       name: docker
       state: present

# Use command module
   - name: Increase virtual memory
     command: sysctl -w vm.max_map_count=262144

# Use sysctl module
   - name: Use more memory
     sysctl:
       name: vm.max_map_count
       value: "262144"
       state: present
       reload: yes

# Use docker_container module
   - name: download and launch a docker elk container
     docker_container:
       name: elk
       image: sebp/elk:761
       state: started
       restart_policy: always
       published_ports:
         - 5601:5601
         - 9200:9200
         - 5044:5044


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology ###

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- What aspect of security do load balancers protect? Load balancers will protect the system from DDoS attacks by shifting the traffic
- What is the advantage of a jump box? To give secure access to a user

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jumpbox and system network.
- What does Filebeat watch for? It monitors log files and events
- What does Metricbeat record? It records the machine metrics

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Webserver| 10.0.0.5   | Linux            |
| Web-2    | Webserver| 10.0.0.7   | Linux            |
| Web-3    | Webserver| 10.0.0.9   | Linux            |
|ELK-Server|Monitoring| 10.1.0.4   | Linux            |





### Access Policies ###

The machines on the internal network are not exposed to the public Internet. 

Only the jump box provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 5061 Kibana Port

Machines within the network can only be accessed by the jump box provisioner.
- Which machine did you allow to access your ELK VM? The user's personal IP address

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Personal IP address  |
| Web-1    | No                  | 10.1.0.4             |
| Web-2    | No                  | 10.1.0.4             |
| Web-3    | No                  | 10.1.0.4             |
|ELK-Server| No                  | 10.1.0.4             |

### Elk Configuration ###

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible is a free open-source tool
- Simple to set up and use. Easy coding skills for highly complex IT workflows
- No extra software is requires so there is extra room for application resources on the server
- The firewalls are already installed in the system so there is no need to set up a separate management structure
- The network environment can easily be deployed anywhere

The playbook implements the following tasks:
- Install docker.io
- Install pip3
- Install Docker python module
- Increase virtual memory
- Download and launch a docker

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(https://drive.google.com/file/d/1PpYTiARgsGNE9UMjSg9j9MgBOy4-k-NK/view?usp=sharing)



### Target Machines & Beats ###
This ELK server is configured to monitor the following machines:
- The webservers: Web-1, Web-2, Web-3

We have installed the following Beats on these machines:
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects data about the file system and metricbeat collects the machine metrics, such as how long the machine has been running. 

### Using the Playbook ###
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to ansible.
- Update the ansible file to include the webservers and the ELK server
- Run the playbook, and navigate to the ELK server to check that the installation worked as expected.

Answer the following questions to fill in the blanks:_
- Which file is the playbook? Where do you copy it? All YAML files are in the /etc/ansible directory
- Which file do you update to make Ansible run the playbook on a specific machine? Hosts file
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? It is specified it the Hosts and YAML files with which machines will do the specified installations
- Which URL do you navigate to in order to check that the ELK server is running? https://[Host IP]:5601/app/kibana#home

As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

Example of creating an ansible playbook:

nano metricbeat.yml

nano metricbeat-config.yml

ansible-playbook metricbeat.yml
