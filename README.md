## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

!project/Diagram.drawio

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the pentest.yml file may be used to install only certain pieces of it, such as Filebeat.

  ---                                                                                                                  
- name: Config Web VM with Docker                                                                                     
  hosts: webservers                                                                                                    
  become: true                                                                                                         
  tasks:                                                                                                               
  - name: docker.io                                                                                                   
    apt:                                                                                                              
        force_apt_get: yes                                                                                                 
        name: docker.io                                                                                                      
        state: present                                                                                                                                                                                                                        

- name: Install pip                                                                                                    
  apt:                                                                                                              
      force_apt_get: yes                                                                                                   
      name: python-pip                                                                                                     
      state: present                                                                                                                                                                                                                        

- name: Install Docker python module                                                                                   
  pip:                                                                                                                   
     name: docker                                                                                                        
     state: present                                                                                                                                                                                                                        

- name: download and launch a docker web container             
  docker_container:                                                                                                     
    name: dvwa                                                                                                           
    image: cyberxsecurity/dvwa                                                                                           
    state: started                                                                                                    
    published_ports: 80:80                                                                                                                                                                                                              

- name: Config elk VM with Docker                                                                                     
  hosts: elkservers                                                                                                    
  remote_user: elk                                                                                                     
  become: true                                                                                                         
  tasks:   

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly avaliable, in addition to restricting traffic to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system monitor.
-What does Filebeat watch for? _it looks out for specify files and locations for monitoring and logging traffic.
-What does Metricbeat record? _It records Metrics from services running on the server and send them to an specify output location.

The configuration details of each machine may be found below.

| Name          | Function       | IP Address     | Operating System |   |
|---------------|----------------|----------------|------------------|---|
| JumpBox       | GateWay        | 104.211.55.190 | linux            |   |
| ELk-VM        | Elk Server     | 40.117.130.214 | Linux            |   |
| DVWA-VM1-NEW  | Back Up Server | 52.170.192.221 | Linux            |   |
| Load Balancer | Load Balancer  | 162.50.50.50   |                  |   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: any

Machines within the network can only be accessed by Jumpbox.
- Which machine did you allow to access your ELK VM? What was its IP address? _The JumpBox (10.0.0.5)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | *                    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible? _it allows for multiple services to be installed and configurated on to connected servers in the network.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring _10.0.0.7, 10.0.0.8, 10.0.0.9

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed _DVWA-VM1-new, jumpbox

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc. _filebeat collects PCF application logs. Metricbeat collects metric from services like Apache, NGINX, and MYSQL.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the pentest.yml file to ansible.
- Update the pentest.yml file to include...
- Run the playbook, and navigate to docker container list -a to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_elk-playbook.yml 
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_host file, you have to add the the ip address to the webservers
- _Which URL do you navigate to in order to check that the ELK server is running? http://[elk ip]:5601
