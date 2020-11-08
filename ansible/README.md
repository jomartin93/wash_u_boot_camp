## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/network_diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, select portions of the playbook (.yml) files may be used to install only certain pieces of it, such as Filebeat.

Ansible Playbook Files:
  (pentest.yml) - install DVWA
  (install-elk.yml) - install ELK Server
  (filebeat-playbook.yml) - install and configure Filebeat on DVWA servers
  (metricbeat-playbook.yml) - install and configure Metricbeat on DVWA servers

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system and system metrics.

The configuration details of each machine may be found below.

|         Name         | Function |                      IP Address                      | Operating System |
|:--------------------:|:--------:|:----------------------------------------------------:|:----------------:|
| Jump_Box_Provisioner | Gateway  | 10.0.0.4 (Private) / <Jump-Box Public IP> (Public)   | Ubuntu 18.04     |
| Web-1                | Server   | 10.0.0.5 (Private) / (No Public)                     | Ubuntu 18.04     |
| Web-2                | Server   | 10.0.0.6 (Private) / (No Public)                     | Ubuntu 18.04     |
| Web-3                | Server   | 10.0.0.7 (Private) / (No Public)                     | Ubuntu 18.04     |
| ELK-SERVER           | Server   | 10.1.0.4 (Private) / <ELK-SERVER Public IP> (Public) | Ubuntu 18.04     |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

<Personal Public IP Address>

Machines within the network can only be accessed by SSH from the Jump Box Machine (10.0.0.4). The ELK-SERVER also has web access from <Personal Puablic IP Address> via HTTP (Port 5601).

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses                                                   |
|----------------------|---------------------|------------------------------------------------------------------------|
| Jump-Box-Provisioner | No                  | Personal Public IP Address (SSH)                                       |
| RedLoadBalancer      | No                  | Personal Public IP Address (HTTP, Port 80)                             |
| Web-1                | No                  | RedLoadBalancer Public IP (HTTP, Port 80), 10.0.0.4 (Jump-Box: SSH)    |
| Web-2                | No                  | RedLoadBalancer Public IP (HTTP, Port 80), 10.0.0.4 (Jump-Box: SSH)    |
| Web-3                | No                  | RedLoadBalancer Public IP (HTTP, Port 80), 10.0.0.4 (Jump-Box: SSH)    |
| ELK-SERVER           | No                  | Personal Public IP Address (HTTP, Port 5601), 10.0.0.4 (Jump-Box: SSH) |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because of the ability to quickly deploy multiple servers and avoid the possibility of misconfiguring machines that comes with manual configuration.

The playbook implements the following tasks:

- Install Docker.io and install Python3
- Install Docker module with pip3
- Increase VM Memory
- Downlaod and launch ELK container

The following screenshot displays the result of running `sudo docker ps` after successfully configuring the ELK instance.

(Images/docker_ps_output.png)

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

|  Name | IP Address |
|:-----:|:----------:|
| Web-1 | 10.0.0.5   |
| Web-2 | 10.0.0.6   |
| Web-3 | 10.0.0.7   |

We have installed the following Beats on these machines:

Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:

Filebeat:
  Filebeat is used to collect and organize log files and collect data on the file system. You would expect syslogs or Apache log files as an output for Filebeat.
Metricbeat
  Metric beat collects metrics about the machine such as CPU or memory usage.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

Filebeat:
- Copy the (filebeat-config.yml) file to /etc/ansible/files.
- Update the (filebeat-config.yml) file to include the ELK-SERVER private IP to line 1105 and 1805.
- Run the ansible-playbook command. Go to http://<ELK-SERVER Public IP>:5601/app/kibana#/home to confirm the installation was succesful.

Metricbeat:
- Copy the metricbeat-config.yml file to /etc/ansible/files.
- Update the metricbeat-config.yml file to include the ELK-SERVER private IP to line 62 and 95.
- Run the ansible-playbook command. Go to http://<ELK-SERVER Public IP>:5601/app/kibana#/home to confirm the installation was succesful.