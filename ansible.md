# Ansible Notes

## Ansible basics

Ansible is simple open source IT engine which automates application deployment, intra service orchestration, cloud provisioning and many other IT tools.

Ansible is easy to deploy because it does not use any agents or custom security infrastructure.

Ansible uses playbook to describe automation jobs, and playbook uses very simple language i.e. YAML (It’s a human-readable data serialization language & is commonly used for configuration files, but could be used in many applications where data is being stored)which is very easy for humans to understand, read and write. Hence the advantage is that even the IT infrastructure support guys can read and understand the playbook and debug if needed (YAML – It is in human readable form).

Ansible does not manage one system at time, it models IT infrastructure by describing all of your systems are interrelated. Ansible is completely agentless which means Ansible works by connecting your nodes through ssh(by default). But if you want other method for connection like Kerberos, Ansible gives that option to you.

After connecting to your nodes, Ansible pushes small programs called as “Ansible Modules”. Ansible runs that modules on your nodes and removes them when finished. Ansible manages your inventory in simple text files (These are the hosts file). Ansible uses the hosts file where one can group the hosts and can control the actions on a specific group in the playbooks.


After connecting to your nodes, Ansible pushes small programs called as “Ansible Modules”. Ansible runs that modules on your nodes and removes them when finished. Ansible manages your inventory in simple text files (These are the hosts file). Ansible uses the hosts file where one can group the hosts and can control the actions on a specific group in the playbooks.


Ansible uses YAML syntax for expressing Ansible playbooks. This chapter provides an overview of YAML. Ansible uses YAML because it is very easy for humans to understand, read and write when compared to other data formats like XML and JSON.

Every YAML file optionally starts with “---” and ends with 


###Understanding YAML
####key-value pair
YAML uses simple key-value pair to represent the data. The dictionary is represented in key: value pair.

Note − There should be space between : and value.

Example: A student record
```
--- #Optional YAML start syntax 
james: 
   name: james john 
   rollNo: 34 
   div: B 
   sex: male 
… #Optional YAML end syntax 
```

#####Abbreviation
You can also use abbreviation to represent dictionaries.

Example

```
James: {name: james john, rollNo: 34, div: B, sex: male}
```

#####Representing List
We can also represent List in YAML. Every element(member) of list should be written in a new line with same indentation starting with “- “ (- and space).

Example
````
countries:  
   - America 
   - China 
   - Canada 
   - Iceland 
...
````
#####Abbreviation
You can also use abbreviation to represent lists.

Example

```
Countries: [‘America’, ‘China’, ‘Canada’, ‘Iceland’] 
```
#####List inside Dictionaries
We can use list inside dictionaries, i.e., value of key is list.

Example

``` 
james: 
   name: james john 
   rollNo: 34 
   div: B 
   sex: male 
   likes: 
      - maths 
      - physics 
      - english 
… 
```

#####List of Dictionaries
We can also make list of dictionaries.

Example

```
- james: 
   name: james john 
   rollNo: 34 
      div: B 
   sex: male 
   likes: 
      - maths 
      - physics 
      - english 

- robert: 
      name: robert richardson 
      rollNo: 53 
      div: B 
      sex: male 
   likes: 
      - biology 
      - chemistry 
…  
```

YAML uses “|” to include newlines while showing multiple lines and “>” to suppress newlines while showing multiple lines. Due to this we can read and edit large lines. In both the cases intendentation will be ignored.

We can also represent Boolean (True/false) values in YAML. where boolean values can be case insensitive.

Example

```
- james: 
   name: james john 
   rollNo: 34 
   div: B 
   sex: male 
   likes: 
      - maths 
      - physics 
      - english 
   
   result: 
      maths: 87 
      chemistry: 45 
      biology: 56 
      physics: 70 
      english: 80 
   
   passed: TRUE 
   
   messageIncludeNewLines: | 
      Congratulation!! 
      You passed with 79% 
   
   messageExcludeNewLines: > 
      Congratulation!! 
      You passed with 79% 
      
```
#####AD Hoc Commands

Ad hoc commands are commands which can be run individually to perform quick functions. These commands need not be performed later.

For example, you have to reboot all your company servers. For this, you will run the Adhoc commands from ‘/usr/bin/ansible’.

These ad-hoc commands are not used for configuration management and deployment, because these commands are of one time usage.

ansible-playbook is used for configuration management and deployment.

#####Parallelism and Shell Commands

Reboot your company server in 12 parallel forks at time. For this, we need to set up SSHagent for connection.

To run reboot for all your company servers in a group, 'abc', in 12 parallel forks −


```
$ Ansible abc -a "/sbin/reboot" -f 12
```
By default, Ansible will run the above Ad-hoc commands form current user account. If you want to change this behavior, you will have to pass the username in Ad-hoc commands as follows −

```
$ Ansible abc -a "/sbin/reboot" -f 12 -u username
```

#####File Transfer
You can use the Ad-hoc commands for doing SCP (Secure Copy Protocol) lots of files in parallel on multiple machines.
######Transferring file to many servers/machines
```
$ Ansible abc -m copy -a "src = /etc/yum.conf dest = /tmp/yum.conf"
```
######Creating new directory
```
$ Ansible abc -m file -a "dest = /path/user1/new mode = 777 owner = user1 group = user1 state = directory" 
```

######Deleting whole directory and files

```
$ Ansible abc -m file -a "dest = /path/user1/new state = absent"
```
#####Managing Packages
The Ad-hoc commands are available for yum and apt. Following are some Ad-hoc commands using yum.

The following command checks if yum package is installed or not, but does not update it.

```
$ Ansible abc -m yum -a "name = demo-tomcat-1 state = present"
```
The following command check the package is not installed.

```
$ Ansible abc -m yum -a "name = demo-tomcat-1 state = absent" 
```
The following command checks the latest version of package is installed.

```
$ Ansible abc -m yum -a "name = demo-tomcat-1 state = latest" 
```
#####Gathering Facts
Facts can be used for implementing conditional statements in playbook. You can find adhoc information of all your facts through the following Ad-hoc command −

```
$ Ansible all -m setup 
```

#####Ansible
Playbooks are the files where Ansible code is written. Playbooks are written in YAML format. YAML stands for Yet Another Markup Language. Playbooks are one of the core features of Ansible and tell Ansible what to execute. They are like a to-do list for Ansible that contains a list of tasks.

######Playbook Structure

