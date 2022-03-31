Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Update the path with the name of your diagram. 
![Project1 Topography](https://user-images.githubusercontent.com/95101213/160952863-bd9917e1-f831-4fc6-bb9a-9e150ae8761f.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the configuration and YAML file may be used to install only certain pieces of it, such as Filebeat.

Enter the playbook file.


This document contains the following details:
Description of the Topology
Access Policies
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build


Description of the Topology:

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly protected, in addition to restricting access to the network.

What aspect of security do load balancers protect? 
Load balancers protect servers and distribute the traffic evenly across the servers and mitigate DoS attacks.

What is the advantage of a jump box?
The jump box is the only way to access the DVWA machines within the local network via SSH key on port 22 making the network secure from external attacks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data  and system logs.

What does Filebeat watch for?
Filebeat monitors the log files or location you specify, collects log events and forwards them to Elasticsearch or Logstash.

What does Metricbeat record?
Metricbeat periodically collects metrics from the operating system and from services running on the server.  Metricbeat then takes the metrics and statistics it collects and ships them to the output you specify, such as Elasticsearch or Logstash.  

The configuration details of each machine may be found below:

| Name         | Function                                              | IP Address | Operating System    |
|--------------|-------------------------------------------------------|------------|---------------------|
| Jump Box     | Gateway                                               | 10.0.0.1   | Linux (ubuntu18.04) |
| Web-1        | Process web content and deliver to users              | 10.0.0.5   | Linux (ubuntu18.04) |
| Web-2        | Process web content and deliver to users              | 10.0.0.6   | Linux (ubuntu18.04) |
| Web-3        | Process web content and deliver to users              | 10.0.0.7   | Linux (ubuntu18.04) |
| Elk Stack VM | Collects and process data from Web-1, Web-2 and Web-3 | 10.1.0.4   | Linux (ubuntu18.04) |

Access Policies:
The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

Add whitelisted IP addresses:
The white listed IP address is 173.29.33.157, my laptop contains the SSH private key to log into the JumpBox Provisioner on port 22.

Machines within the network can only be accessed by the Jump Box Provisioner.
Which machine did you allow to access your ELK VM? What was its IP address?_
The Jump Box Provisioner has access to the ELK VM  with a private IP address of 10.0.0.4 using a SSH on port 22.
The local host (my laptop) has access to the ELK VM with a public IP address of  173.29.33.157 using a TCP on port 5601.




A summary of the access policies in place can be found in the table below.
| Name                  	| Publicly Accessible 	| Allowed IP Addresses              	|
|-----------------------	|---------------------	|-----------------------------------	|
| Jump Box Provisioner  	| Yes                 	| 173.29.33.157                     	|
| Load Balancer         	| Yes                 	| 173.29.33.157                     	|
| Web-1                 	| No                  	| 10.0.0.4, 10.1.0.4, 52.234.45.199 	|
| Web-2                 	| No                  	| 10.0.0.4, 10.1.0.4, 52.234.45.199 	|
| Web-3                 	| No                  	| 10.0.0.4, 10.1.0.4, 52.234.45.199 	|
| Elk-VM                	| Yes                 	| 173.29.33.157                     	|


Elk Configuration:
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

What is the main advantage of automating configuration with Ansible?
Ansible is a provisioner that is utilized to automatically deliver infrastructure as a code, which drastically reduces the potential for human error and simplifies the process of configuring potentially thousands of machines identically all at once.

The playbook implements the following tasks:
In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
Configure Elk VM with Docker on the elk host with the remote user azdmin
Install docker.io, Install python3-pip, Install Python Docker module
Increase virtual memory
Use more memory
Download and launch a docker elk container on restart
Enable service docker on boot
     
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Update the path with the name of your screenshot of docker ps output.
![Elk Docker ps command](https://user-images.githubusercontent.com/95101213/160952972-06f06028-ed48-488e-8e0f-cea70650fffe.PNG)

Target Machines & Beats:
This ELK server is configured to monitor the following machines:
Web-1, IP Address 10.0.0.5
Web-2, IP Address 10.0.0.6
Web-3, IP Address 10.0.0.7

We have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat consists of two main components “inputs” and “harvesters” which work together to tail files and send event data to the output you specify. The harvester is responsible for reading the content of a single file. The input is responsible for managing the harvesters and finding all sources to read from. 
Metricbeat collects metrics from the operating system and services running on the servers.  It takes the metrics and statistics that it collects and ships them to the output you specify, such as Elasticsearch or Logstash.  


Using the Playbook:
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
Copy the filebeat-config.yml  file to root@6f07c4aa4ec4:/etc/ansible/files.filebeat-config.yml
Update the ansible hosts file to include the following webservers:
 [webservers]
10.0.0.5 ansible_python_interpreter=/usr/bin/python3
10.0.0.6 ansible_python_interpreter=/usr/bin/python3
10.0.0.7 ansible_python_interpreter=/usr/bin/python3

[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3

Run the playbook, and navigate to http://20.228.251.217:5601/app/kibana#/home to check that the installation worked as expected.

Answer the following questions to fill in the blanks:

Which file is the playbook? 
filebeat-playbook.yml is the playbook.

Where do you copy it?
The filebeat playbook is copied inside of the Jump Box Provisioner VM- root@6f07c4aa4ec4:/etc/ansible/roles/filebeat-playbook.yml

Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

The root@6f07c4aa4ec4:/etc/ansible/hosts file has to be updated in order to run the playbook on a specific machine.
Once you navigate into the /etc/ansible/hosts, nano into the hosts file and add the [webservers] that will be configured with filebeat and the [elk] VM which will be configured with the ELK Stack. 

Which URL do you navigate to in order to check that the ELK server is running?
http://20.228.251.217:5601/app/kibana#/home

As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc:

ssh azdmin@jump-box-ip-address 
sudo docker container list -a (command list the name of the ansible container)
sudo docker start [add container name] 
sudo docker attach [add container name] (command will move you into the ansible container)
cd /etc/ansible
nano /etc/ansible/hosts (once inside of the hosts file configure [webservers] and [elk] )
 curl https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb > /etc/filebeat/filebeat.yml
dpkg -i filebeat-7.4.0-amd64.deb
nano filebeat-playbook.yml (add the filebeat playbook configuration)
ansible-playbook filebeat-playbook.yml

