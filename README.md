## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/justinsiple/UofM-CBSexperience/blob/main/Diagrams/Network.drawio

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above or select portions of the file may be used to install only certain pieces of it, such as Filebeat.

  https://github.com/justinsiple/UofM-CBSexperience/blob/main/Ansible/filebeat-playbook.yml.txt

This document contains the following details:
- Description of the network with diagram
- Docker configuration for the VM's
- Docker configuration for the ELK network
  - Filebeat and metricbeat yaml playbooks 
- How to Use the Ansible Build
- Each machine in use


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA (Damn Vulnerable Web Application).

Load balancing ensures that the application will be highly optimized, in addition to restricting open access to the network.
To this point, a load balancer can help protect our network from DOS attacks by spreading any requests out over a larger processing ability.
They also can be set up with inbound and outbound rules in place that will help to restrict what types of traffic are allowed thus 
limiting what a hacker could gain control of.  

Integrating an ELK server allows users to easily monitor the vulnerabilities of VMs for changes to the files and system metrics.
Filebeat watches all of our log files and events and sends them to Elasticsearch for indexing. 
Metricbeat does this same task with the computer metric running on our system. 

The configuration details of each machine may be found below.


| NAME       | FUNCTION   | IP       | OS    |
|------------|------------|----------|-------|
| Jump Box   | Gateway    | 10.0.0.4 | Linux |
| Web 1      | Employee   | 10.0.0.5 | Linux |
| Web 2      | Employee   | 10.0.0.6 | Linux |
| ELK Server | Monitoring | 10.1.0.4 | Linux |



Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the load balancer and jumpbox machines can accept connections from the Internet. Access to these machines is only allowed from the following IP addresses:
The jumpbox only from IP 24.179.181.32
The load balancer from IP 24.179.181.32

Machines within the network can only be accessed by the jump box.
The ELK server also can only be accessed by the jump box over ssh via IP 10.0.0.4

A summary of the access policies in place can be found in the table below.

| NAME          | PUBLIC ACCESS | ALLOWED IP'S            |
|---------------|---------------|-------------------------|
| Jump box      | Yes           | 24.179.181.32           |
| ELK server    | No            | 20.211.165.109 10.0.0.4 |
| Load balancer | Yes           | All through port 80     |
| Web-1         | No            | 20.227.0.189            |
| Web-2         | No            | 20.227.0.189            |




### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this way we can 
spin up multiple machines very quickly and we can be assured they are all done exactly the same way. 

The playbook implements the following tasks:
- First we determine whick hosts this playbook will be run on and in this case we have named that host elk. 
- The first play then is to install docker.io
- The second play will install pip3 which will assist in our next step (three) that installs Docker python module.
- Next we need to make sure there is enough memory reserved for this to collect all the information it will have to store and that's what the fourth play is.      
- We then download and launch the elk container. 
- Finally we add a play to start docker automatically each time we boot up.   

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/justinsiple/UofM-CBSexperience/blob/main/Diagrams/Docker%20containers%20open.png



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1
- Web-2

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is used to collect log data and send it to our ELK configuration so we can analyze it more easily. Filebeat will send us things like system logs, error logs, and user logs, as some examples. 
- Metricbeat is used to collect metric data and send it to our ELK configuration so we can alayze it more easily. Metricbeat will send us things like CPU and memory usage, as well as monitoring information on services such as Apache or Nginx as examples.   



### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-ansible.yml and metricbeat-ansible.yml files to the "files" directory in the "Ansible" directory.
- Update the hosts file to include "[local IP] ansible_python_interpreter=/usr/bin/python3" for each IP you wish to have this on. 
- Run the playbook, and navigate to "http://[your.VM.IP]:5601/app/kibana" to check that the installation worked as expected.

- The playbooks are files "filebeat-ansible.yml" and "metricbeat-ansible.yml" and are saved in ~/etc/ansible
- The file and location of that hosts file that needs to be updated to run the playbooks and ELK server on specific machines is ~/etc/ansible/hosts. To then determine with machines each playbook will be run on we will add those local IP addresses under the [webserver] or [elk] headings as shown in the attached screen shot. 

https://github.com/justinsiple/UofM-CBSexperience/blob/main/Diagrams/Hosts%20file%20updates%20for%20playbooks.png

- To ensure everything is running properly navigate to http://[your.VM.IP]:5601/app/kibana

