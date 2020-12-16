# ELK-Stack-Project
Elk Stack Project Housed In Cloud Via Microsoft Azure
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/Ryanhaug1/ELK-Stack-Project/blob/main/Diagrams/Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - ['Install-elk.yml'](Ansible/install_elk.yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, improve availability in addition to restricting traffic to the network.
-   The function of load balancing is to offset any increase in traffic to be shared between more than just a single server.

-   A jump box creates the ability to access the internal network through a single node that is public facing. 
	Ensuring that even though most of the devices are housed in the cloud, not all devices need to have a public IP address. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics.
-   Filebeat monitors for any changed information in the file system.

The configuration details of each machine may be found below.

| Name                 | Function | IP Address | Operating System |
|----------------------|----------|------------|------------------|
| Jump-Box-Provisioner | Gateway  | 10.0.0.4   | Linux            |
| Web-1                | Server   | 10.0.0.5   | Linux            |
| Web-2                | Server   | 10.0.0.6   | Linux            |
| ELK-VM               | Server   | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
-   My home IP address (47.199.43.192)
	
Machines within the network can only be accessed by connecting through the Jumpbox.
-   Jump-Box-Provisioner: 10.0.0.4
	Local Workstation via SSH IP 168.62.200.69
 
A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses |
|----------------------|---------------------|----------------------|
| Jump-Box-Provisioner | Yes                 | Home IP              |
| Web-1                | No                  | 10.0.0.4             |
| Web-2                | No                  | 10.0.0.4             |
| ELK-VM               | No                  | Home IP, 10.1.0.4    |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
-   The biggest advantage to using a single playbook to execute commands on multiple servers.

The playbook implements the following tasks:


    - name: docker.io install
      apt:
        update_cache: yes
        name: docker.io
        state: present

    - name: Install Pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    - name: Install Python Docker Module
      pip:
        name: docker
        state: present

    - name: Increase Virtual Memory
      command: sysctl -w vm.max_map_count=262144

    - name: Verify Increase of Memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

    - name: Download and Launch Docker ELK Container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044



The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/Ryanhaug1/ELK-Stack-Project/blob/main/Diagrams/docker_ps_output.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-   Web-1 10.0.0.5
    Web-2 10.0.0.6

We have installed the following Beats on these machines:
-   Filebeat

These Beats allow us to collect the following information from each machine:
-   Filebeat is a lightweight shipper that is used for forwarding and centralizing log data. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible/.
- Update host file to include [webservers] w/ Web-1/Web-2 IP addresses & [elk] w/ ELK-VM IP address
- Run the playbook, and navigate to command line to check that the installation worked as expected.
- Use the install-elk.yml playbook
- Edit /etc/ansible/host file to add webserver/elk with the device IP addresses. 
- Edit install-elk.yml  and on the hosts line put the relevant host name to install ELK
- Edit filebeat-playbook.yml and on the hosts line put the relevant host name to install Filebeat 
- The URL that can be used to see the Kibana web application running on the ELK server:
   Use the public IP address of the ELK VM - http://[ELK.VM.Public.IP]:5601/app/kibana
