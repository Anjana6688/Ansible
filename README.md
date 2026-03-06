# Ansible
/***************************Session-22***************************************/
Topics:   
        
		Disadvantages of shell
        What is configuration management
        Push vs Pull
        Ansible installation
        Ansible commands
        XML vs JSON vs YAML
        Playbooks
/***************************************************************************/

How to connect one linux server to other:  
1.login to the server and connect another server using below command
ssh ec2-user@10.36.85.1 'touch /tmp/shell.txt'
so it will login without interactively.
Also we can use SCP command to transfer files.  
2. copy the script and dependent files, mongo.repo and mongodb.sh
command: scp mongo.repo mongodb.sh ec2-user@10.36.86.1:/tmp/shell  
3. then execute from main server
ssh ec2-user@10.36.86.1 'cd /tmp&& sudo sh mongodb.sh

Why Ansible?
============
Ansible is developed in python.
Ansible is used for configuration.Configuration is used for within the server.
Ansible can connect any system aws,azure windows linux/mac server by modules.
If we have 500+ servers we need to write complex scripts and need to monitor parallely to take logs so to overcome tools are generated.Those are configuration Tools.
To Handle all these we are using push and pull(Ansible is popular in Push and recently impelemented Pull(For Pull chef is popular) As well.


What is pull and psuh?
=====================    
Case:   Your friend sent an parcel via DDTC  
Polling/Pulling
===============
I go to DTDC everyday and check whether I received courier or not
Event Driven/Push
===============
You carry on, when you get courier it will be delivered.
Event driven like Bank will automatically sent updates when amount debit/credited.
For this we will save lot of time and systems are looslu coupled in Push systems.


Pull Vs Push Architecture:
==========================
Pull:
=====
  
In Pull Architecture we need nodes with agent software to pull Configurations/connect to Configuration server.
So we need to install agent software in every node and configured like it will check every configuration every 30mins.
SO Agent software required and own protocal to pull there is no SSH.
There is Ruby language for this setup.
      
Push:
=====
   
In Push Architecture there is no need extra software.In Configuration server it will automaticall push to noded.
It will connect nodes to SSH.SO it is secure.
So Ansible(CLI) is popular in it and files are Yaml Language,which is compartively simple when compare to Ruby.
Master node:

ansible all -i 172.31.26.71, -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ping


Data Transfer Objects :JSON,YAML,XML
These will transfer the data btwn systems.   

XML -> Extensive Markup Language
================================   

<Name>Sivakumar</Name>
JSON:
{
"name:" "anjana",

}
    
YAML:Yet another markup language
=====================
starts with - and here adress is string/list so again starts with - and all should be in same line
like name dob...

- name: "Anjana"
  dob: "01/06/1998"
  age:21
  addresses:
  - doorNo: 2-216
    Street: "Taimoor nagar"
	skills:
	- java
	- python
	
	here each one is a list so we used -.    
	
How to write an Ansible Playbook?
=================================   
Inventory:
In this file we can specify multiple servers in a group so if we define with that group in  playbook it will connect those servers are under that group.
suppose we have created a group [frontend] under this we have multiple servers if you want to connetct frontend it will connect serveres under that group.

Take two serveres one is Ansible installed and other one is normal.
1. In Ansible Server install Ansible ---> sudo dnf install ansible - you
2. To connect server ---> ansible all -i 44.200.45.27, -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ping
i refers inventory ,all referes all ip address ,dnf referes aruguments

Ex:
[ ec2-user@ip-172-31-79-4 ~ ]$ ansible all -i 44.200.45.27, -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ping
44.200.45.27 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

we give ping in response we got pong.we used ping command to accept all fire wall connection.

3. To install any other software use
ansible all -i 44.200.45.27, -e ansible_user=ec2-user -e ansible_password=DevOps321 -b -m dnf -a "name-nginx state=installed"

 -m refers module so if you want to run as sudo before starting module use -b which means become sudo, -a means arguments these are arguments name-nginx state=installed"
Ex:[ ec2-user@ip-172-31-79-4 ~ ]$ ansible all -i 44.200.45.27, -e ansible_user=ec2-user -e ansible_password=DevOps321 -b -m dnf -a "name=nginx state=installed"
44.200.45.27 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": true,
    "msg": "",
    "rc": 0,
    "results": [
        "Installed: nginx-core-2:1.20.1-22.el9_6.3.x86_64",
        "Installed: nginx-filesystem-2:1.20.1-22.el9_6.3.noarch",
        "Installed: redhat-logos-httpd-90.5-1.el9_6.1.noarch",
        "Installed: nginx-2:1.20.1-22.el9_6.3.x86_64"
    ]
}
 
 4. To start the nginx
 ansible all -i 44.200.45.27, -e ansible_user=ec2-user -e ansible_password=DevOps321 -b -m service -a "name=nginx state=started"

Even linux we are using commands which are using above so for multiple thing we can't run each command.so we are using Modules in Ansible called playbook.
How to run playbook?
we have written same logic in 
ansible-playbook -i Inventory.ini -e ansible_user=ec2-user -e ansible_password=DevOps321 01.playbook.yaml

[ ec2-user@ip-172-31-79-4 ~/Ansible ]$ ansible-playbook -i Inventory.ini -e ansible_user=ec2-user -e ansible_password=DevOps321 01.playbook.yaml

PLAY [To ping the server] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [44.200.45.27]

TASK [ping the server] ***************************************************************************************************************************
ok: [44.200.45.27]

PLAY RECAP ***************************************************************************************************************************************
44.200.45.27               : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

everything is avialble in ansible documentation module.
if you want to intsall packages search with ansible install packages.
or ansible 


