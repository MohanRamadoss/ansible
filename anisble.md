http://devopstechie.com/install-and-configure-ansible-tower-centos7/

https://github.com/PacktPublishing/Implementing-DevOps-with-Ansible-2

runtech  -tool 

https://github.com/codespaces-io
https://github.com/codespaces-io/codespaces to run the vm

https://github.com/schoolofdevops/ansible-bootcamp-code


https://www.schoolofdevops.com/blog/tutorial-how-to-write-ansible-playbooks/

https://www.schoolofdevops.com/blog/tag/ansible/

https://schoolofdevops.slides.com/schoolofdevops/creating-roles-64#/25



setup the facts to create a custom discovery 


What is Ansible? 
Ansible is open-source automation and orchestration engine. In other words, it includes all necessary IT automation tools.

It is a very simple yet powerful tool to use: it does not requires hours long setup for managing your servers, it is scalable, it is secure, it is easy to understand.

How does it work? 
Ansible works by connecting your remote servers via SSH, and will push out small programs which are xecuted by Ansible.
These small programs will be the desired state of the infrastructure conﬁguration. 
Ansible then executes these programs over SSH and deletes them when ﬁnished.


Why SSH? 
Using SSH brings many features out-of-the-box and solves bucket-load of problems. 
Here are few of them: 
• Agentless : No server-side software or agent required,
 you only need OpenSSH daemon.
 • Secure: It is a very secure protocol. 
 • Resource Utilization: When Ansible is not managing remote server there are no resources consumed on those servers. 
 • Turn-Key: Allows you to start managing your machines immediately. 
 • Credential Segregation: It uses existing SSH credentials, as well as your privileges. For instance, if a developer is not allow to access to production server 
   he will not be able to push contents to that server.


How to use it? 
Using Ansible is pretty straight forwards actually, 
however you should posses some knowledge of :
 • SSH and basic bash 
 • Working with the command line 
 
   
Master server is the control node 

192.168.43.20 master.rmohan.com  master 







ere is the steps for ansible controller : 
------------------------------------------

 yum update

yum install epel-release
= Error

 browser for epel-release for redhat 7

 yum install wget 

 wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

 ls 

 rpm -i epel-release-latest-7.noarch.rpm

 yum install ansible -y

 ansible --version
---
root  adduser ansible
root  passwd ansible
password: ansible


root  visudo
	root ALL = (ALL) ALL
	ansible ALL = (ALL) NOPASSWD: ALL

root  vi /etc/ssh/sshd-config
PasswordAuthentication yes
#PasswordAuthenticatin no

root  service sshd restart
root  exit






[root@master ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.43.20 master.rmohan.com  master
192.168.43.21 node1.rmohan.com  node1
192.168.43.22 node2.rmohan.com  node2
192.168.43.23 node3.rmohan.com  node3
192.168.43.40 demo.rmohan.com   demo
192.168.43.41 demo1.rmohan.com  demo1
[root@master ~]# clear
[root@master ~]#  yum install ansible
Loaded plugins: fastestmirror, langpacks
base                                                                                                                                         | 3.6 kB  00:00:00
extras                                                                                                                                       | 3.4 kB  00:00:00
updates                                                                                                                                      | 3.4 kB  00:00:00
(1/2): extras/7/x86_64/primary_db                                                                                                            | 185 kB  00:00:00
(2/2): updates/7/x86_64/primary_db                                                                                                           | 6.9 MB  00:00:01
Determining fastest mirrors
 * base: mirror.0x.sg
 * extras: mirror.0x.sg
 * updates: mirror.0x.sg
Resolving Dependencies
-- Running transaction check
--- Package ansible.noarch 0:2.4.2.0-2.el7 will be installed
-- Processing Dependency: sshpass for package: ansible-2.4.2.0-2.el7.noarch
-- Processing Dependency: python2-jmespath for package: ansible-2.4.2.0-2.el7.noarch
-- Processing Dependency: python-setuptools for package: ansible-2.4.2.0-2.el7.noarch
-- Processing Dependency: python-passlib for package: ansible-2.4.2.0-2.el7.noarch
-- Processing Dependency: python-paramiko for package: ansible-2.4.2.0-2.el7.noarch
-- Processing Dependency: python-jinja2 for package: ansible-2.4.2.0-2.el7.noarch
-- Processing Dependency: python-httplib2 for package: ansible-2.4.2.0-2.el7.noarch
-- Processing Dependency: python-cryptography for package: ansible-2.4.2.0-2.el7.noarch
-- Processing Dependency: PyYAML for package: ansible-2.4.2.0-2.el7.noarch
-- Running transaction check
--- Package PyYAML.x86_64 0:3.10-11.el7 will be installed
-- Processing Dependency: libyaml-0.so.2()(64bit) for package: PyYAML-3.10-11.el7.x86_64
--- Package python-httplib2.noarch 0:0.9.2-1.el7 will be installed
--- Package python-jinja2.noarch 0:2.7.2-2.el7 will be installed
-- Processing Dependency: python-babel = 0.8 for package: python-jinja2-2.7.2-2.el7.noarch
-- Processing Dependency: python-markupsafe for package: python-jinja2-2.7.2-2.el7.noarch
--- Package python-paramiko.noarch 0:2.1.1-4.el7 will be installed
-- Processing Dependency: python2-pyasn1 for package: python-paramiko-2.1.1-4.el7.noarch
--- Package python-passlib.noarch 0:1.6.5-2.el7 will be installed
--- Package python-setuptools.noarch 0:0.9.8-7.el7 will be installed
-- Processing Dependency: python-backports-ssl_match_hostname for package: python-setuptools-0.9.8-7.el7.noarch
--- Package python2-cryptography.x86_64 0:1.7.2-1.el7_4.1 will be installed
-- Processing Dependency: python-idna = 2.0 for package: python2-cryptography-1.7.2-1.el7_4.1.x86_64
-- Processing Dependency: python-cffi = 1.4.1 for package: python2-cryptography-1.7.2-1.el7_4.1.x86_64
-- Processing Dependency: python-ipaddress for package: python2-cryptography-1.7.2-1.el7_4.1.x86_64
-- Processing Dependency: python-enum34 for package: python2-cryptography-1.7.2-1.el7_4.1.x86_64
--- Package python2-jmespath.noarch 0:0.9.0-3.el7 will be installed
--- Package sshpass.x86_64 0:1.06-2.el7 will be installed
-- Running transaction check
--- Package libyaml.x86_64 0:0.1.4-11.el7_0 will be installed
--- Package python-babel.noarch 0:0.9.6-8.el7 will be installed
--- Package python-backports-ssl_match_hostname.noarch 0:3.4.0.2-4.el7 will be installed
-- Processing Dependency: python-backports for package: python-backports-ssl_match_hostname-3.4.0.2-4.el7.noarch
--- Package python-cffi.x86_64 0:1.6.0-5.el7 will be installed
-- Processing Dependency: python-pycparser for package: python-cffi-1.6.0-5.el7.x86_64
--- Package python-enum34.noarch 0:1.0.4-1.el7 will be installed
--- Package python-idna.noarch 0:2.4-1.el7 will be installed
--- Package python-ipaddress.noarch 0:1.0.16-2.el7 will be installed
--- Package python-markupsafe.x86_64 0:0.11-10.el7 will be installed
--- Package python2-pyasn1.noarch 0:0.1.9-7.el7 will be installed
-- Running transaction check
--- Package python-backports.x86_64 0:1.0-8.el7 will be installed
--- Package python-pycparser.noarch 0:2.14-1.el7 will be installed
-- Processing Dependency: python-ply for package: python-pycparser-2.14-1.el7.noarch
-- Running transaction check
--- Package python-ply.noarch 0:3.4-11.el7 will be installed
-- Finished Dependency Resolution

Dependencies Resolved

====================================================================================================================================================================
 Package                                                   Arch                         Version                                 Repository                     Size
====================================================================================================================================================================
Installing:
 ansible                                                   noarch                       2.4.2.0-2.el7                           extras                        7.6 M
Installing for dependencies:
 PyYAML                                                    x86_64                       3.10-11.el7                             base                          153 k
 libyaml                                                   x86_64                       0.1.4-11.el7_0                          base                           55 k
 python-babel                                              noarch                       0.9.6-8.el7                             base                          1.4 M
 python-backports                                          x86_64                       1.0-8.el7                               base                          5.8 k
 python-backports-ssl_match_hostname                       noarch                       3.4.0.2-4.el7                           base                           12 k
 python-cffi                                               x86_64                       1.6.0-5.el7                             base                          218 k
 python-enum34                                             noarch                       1.0.4-1.el7                             base                           52 k
 python-httplib2                                           noarch                       0.9.2-1.el7                             extras                        115 k
 python-idna                                               noarch                       2.4-1.el7                               base                           94 k
 python-ipaddress                                          noarch                       1.0.16-2.el7                            base                           34 k
 python-jinja2                                             noarch                       2.7.2-2.el7                             base                          515 k
 python-markupsafe                                         x86_64                       0.11-10.el7                             base                           25 k
 python-paramiko                                           noarch                       2.1.1-4.el7                             extras                        268 k
 python-passlib                                            noarch                       1.6.5-2.el7                             extras                        488 k
 python-ply                                                noarch                       3.4-11.el7                              base                          123 k
 python-pycparser                                          noarch                       2.14-1.el7                              base                          104 k
 python-setuptools                                         noarch                       0.9.8-7.el7                             base                          397 k
 python2-cryptography                                      x86_64                       1.7.2-1.el7_4.1                         updates                       502 k
 python2-jmespath                                          noarch                       0.9.0-3.el7                             extras                         39 k
 python2-pyasn1                                            noarch                       0.1.9-7.el7                             base                          100 k
 sshpass                                                   x86_64                       1.06-2.el7                              extras                         21 k

Transaction Summary
====================================================================================================================================================================
Install  1 Package (+21 Dependent packages)

Total download size: 12 M
Installed size: 60 M
Is this ok [y/d/N]: y
Downloading packages:
(1/22): libyaml-0.1.4-11.el7_0.x86_64.rpm                                                                                                    |  55 kB  00:00:00
(2/22): PyYAML-3.10-11.el7.x86_64.rpm                                                                                                        | 153 kB  00:00:00
(3/22): python-backports-1.0-8.el7.x86_64.rpm                                                                                                | 5.8 kB  00:00:00
(4/22): python-backports-ssl_match_hostname-3.4.0.2-4.el7.noarch.rpm                                                                         |  12 kB  00:00:00
(5/22): python-cffi-1.6.0-5.el7.x86_64.rpm                                                                                                   | 218 kB  00:00:00
(6/22): python-babel-0.9.6-8.el7.noarch.rpm                                                                                                  | 1.4 MB  00:00:00
(7/22): python-enum34-1.0.4-1.el7.noarch.rpm                                                                                                 |  52 kB  00:00:00
(8/22): python-ipaddress-1.0.16-2.el7.noarch.rpm                                                                                             |  34 kB  00:00:00
(9/22): python-idna-2.4-1.el7.noarch.rpm                                                                                                     |  94 kB  00:00:00
(10/22): python-markupsafe-0.11-10.el7.x86_64.rpm                                                                                            |  25 kB  00:00:00
(11/22): python-jinja2-2.7.2-2.el7.noarch.rpm                                                                                                | 515 kB  00:00:00
(12/22): ansible-2.4.2.0-2.el7.noarch.rpm                                                                                                    | 7.6 MB  00:00:01
(13/22): python-httplib2-0.9.2-1.el7.noarch.rpm                                                                                              | 115 kB  00:00:00
(14/22): python-paramiko-2.1.1-4.el7.noarch.rpm                                                                                              | 268 kB  00:00:00
(15/22): python-passlib-1.6.5-2.el7.noarch.rpm                                                                                               | 488 kB  00:00:00
(16/22): python-pycparser-2.14-1.el7.noarch.rpm                                                                                              | 104 kB  00:00:00
(17/22): python-ply-3.4-11.el7.noarch.rpm                                                                                                    | 123 kB  00:00:00
(18/22): python2-pyasn1-0.1.9-7.el7.noarch.rpm                                                                                               | 100 kB  00:00:00
(19/22): python-setuptools-0.9.8-7.el7.noarch.rpm                                                                                            | 397 kB  00:00:00
(20/22): sshpass-1.06-2.el7.x86_64.rpm                                                                                                       |  21 kB  00:00:00
(21/22): python2-jmespath-0.9.0-3.el7.noarch.rpm                                                                                             |  39 kB  00:00:00
(22/22): python2-cryptography-1.7.2-1.el7_4.1.x86_64.rpm                                                                                     | 502 kB  00:00:00
--------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                               4.4 MB/s |  12 MB  00:00:02
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : python2-pyasn1-0.1.9-7.el7.noarch                                                                                                               1/22
  Installing : python-httplib2-0.9.2-1.el7.noarch                                                                                                              2/22
  Installing : python-enum34-1.0.4-1.el7.noarch                                                                                                                3/22
  Installing : python-ipaddress-1.0.16-2.el7.noarch                                                                                                            4/22
  Installing : libyaml-0.1.4-11.el7_0.x86_64                                                                                                                   5/22
  Installing : PyYAML-3.10-11.el7.x86_64                                                                                                                       6/22
  Installing : python-backports-1.0-8.el7.x86_64                                                                                                               7/22
  Installing : python-backports-ssl_match_hostname-3.4.0.2-4.el7.noarch                                                                                        8/22
  Installing : python-setuptools-0.9.8-7.el7.noarch                                                                                                            9/22
  Installing : python-babel-0.9.6-8.el7.noarch                                                                                                                10/22
  Installing : python-passlib-1.6.5-2.el7.noarch                                                                                                              11/22
  Installing : python-ply-3.4-11.el7.noarch                                                                                                                   12/22
  Installing : python-pycparser-2.14-1.el7.noarch                                                                                                             13/22
  Installing : python-cffi-1.6.0-5.el7.x86_64                                                                                                                 14/22
  Installing : python-markupsafe-0.11-10.el7.x86_64                                                                                                           15/22
  Installing : python-jinja2-2.7.2-2.el7.noarch                                                                                                               16/22
  Installing : python-idna-2.4-1.el7.noarch                                                                                                                   17/22
  Installing : python2-cryptography-1.7.2-1.el7_4.1.x86_64                                                                                                    18/22
  Installing : python-paramiko-2.1.1-4.el7.noarch                                                                                                             19/22
  Installing : python2-jmespath-0.9.0-3.el7.noarch                                                                                                            20/22
  Installing : sshpass-1.06-2.el7.x86_64                                                                                                                      21/22
  Installing : ansible-2.4.2.0-2.el7.noarch                                                                                                                   22/22
  Verifying  : python-jinja2-2.7.2-2.el7.noarch                                                                                                                1/22
  Verifying  : python-backports-ssl_match_hostname-3.4.0.2-4.el7.noarch                                                                                        2/22
  Verifying  : sshpass-1.06-2.el7.x86_64                                                                                                                       3/22
  Verifying  : python-setuptools-0.9.8-7.el7.noarch                                                                                                            4/22
  Verifying  : python2-cryptography-1.7.2-1.el7_4.1.x86_64                                                                                                     5/22
  Verifying  : python2-jmespath-0.9.0-3.el7.noarch                                                                                                             6/22
  Verifying  : python-idna-2.4-1.el7.noarch                                                                                                                    7/22
  Verifying  : python-markupsafe-0.11-10.el7.x86_64                                                                                                            8/22
  Verifying  : python-ply-3.4-11.el7.noarch                                                                                                                    9/22
  Verifying  : python-passlib-1.6.5-2.el7.noarch                                                                                                              10/22
  Verifying  : python-babel-0.9.6-8.el7.noarch                                                                                                                11/22
  Verifying  : python-paramiko-2.1.1-4.el7.noarch                                                                                                             12/22
  Verifying  : python-backports-1.0-8.el7.x86_64                                                                                                              13/22
  Verifying  : python-cffi-1.6.0-5.el7.x86_64                                                                                                                 14/22
  Verifying  : python-pycparser-2.14-1.el7.noarch                                                                                                             15/22
  Verifying  : libyaml-0.1.4-11.el7_0.x86_64                                                                                                                  16/22
  Verifying  : ansible-2.4.2.0-2.el7.noarch                                                                                                                   17/22
  Verifying  : python-ipaddress-1.0.16-2.el7.noarch                                                                                                           18/22
  Verifying  : python-enum34-1.0.4-1.el7.noarch                                                                                                               19/22
  Verifying  : python-httplib2-0.9.2-1.el7.noarch                                                                                                             20/22
  Verifying  : python2-pyasn1-0.1.9-7.el7.noarch                                                                                                              21/22
  Verifying  : PyYAML-3.10-11.el7.x86_64                                                                                                                      22/22

Installed:
  ansible.noarch 0:2.4.2.0-2.el7

Dependency Installed:
  PyYAML.x86_64 0:3.10-11.el7                  libyaml.x86_64 0:0.1.4-11.el7_0                                  python-babel.noarch 0:0.9.6-8.el7
  python-backports.x86_64 0:1.0-8.el7          python-backports-ssl_match_hostname.noarch 0:3.4.0.2-4.el7       python-cffi.x86_64 0:1.6.0-5.el7
  python-enum34.noarch 0:1.0.4-1.el7           python-httplib2.noarch 0:0.9.2-1.el7                             python-idna.noarch 0:2.4-1.el7
  python-ipaddress.noarch 0:1.0.16-2.el7       python-jinja2.noarch 0:2.7.2-2.el7                               python-markupsafe.x86_64 0:0.11-10.el7
  python-paramiko.noarch 0:2.1.1-4.el7         python-passlib.noarch 0:1.6.5-2.el7                              python-ply.noarch 0:3.4-11.el7
  python-pycparser.noarch 0:2.14-1.el7         python-setuptools.noarch 0:0.9.8-7.el7                           python2-cryptography.x86_64 0:1.7.2-1.el7_4.1
  python2-jmespath.noarch 0:0.9.0-3.el7        python2-pyasn1.noarch 0:0.1.9-7.el7                              sshpass.x86_64 0:1.06-2.el7

Complete!
[root@master ~]# ssh-copy-id demo1.rmohan.com  ; ssh-copy-id node2.rmohan.com 



# Display Ansible command line options available
$ ansible --help
# Show the Ansible version number
$ ansible --version



ansible  -m web  -m ping all

 -a "whoami" all

 -m, –module-name: Module name to execute
-a, –args: Module arguments
-s, –sudo: Run operations with sudo
-k, –ask-pass: Ask for SSH password
-K, –ask-sudo-pass: Ask for sudo password
-u, –user: Connect remote target with different user name
-f, –forks: Specify number of parallel processes to use when communicating with remote 



Copying File

ansible -i web, -m copy -a 'src=myfile dest=/tmp/mynewfile.txt' all

ansible -i web, -m copy -a 'src=myfile dest=/tmp/mynewfile.txt mode=644 owner=jane' all


ansible -i app, -s -m get_url -a 'url=http://apache.imsam.info/ tomcat/tomcat-8/v8.0.3/bin/apache-tomcat-8.0.3.tar.gz dest=/tmp/apache-tomcat-8.0.3.tar.gz' all

2.3.2 Managing Packages
Ansible Master/ Ansible Controller
---------------------------------
 ansible -m ping all
 ansible all -m ping
  ansible all -m shell -a "uptime"

ad-hoc commands:
----------------
 ansible all -m command -a "whoami"
 ansible all -m command -a "pwd"
 
ansible all -b -m command -a "yum install tree -y"
ansible -b localhost -m yum -a "name=git"
ansible -b localhost -m yum -a "name=git state=removed"
ansible -b app -i common-hosts -m yum -a "name=wget"

ansible all -m setup | more
ansible all -m setup -a 'filter=*'

ansible web -m file -a 'path=/etc/fstab'
ansible web -m file -a 'path=/etc/'
ansible web -s -m file -a 'path=/tmp/etc/ state=directory mode=0700 owner=root' --> creates a directory with 0700 permission to root user
ansible web -s -m copy -a 'src=/etc/fstab dest=/tmp/etc/fstab'-->copying a file with copy module
ansible web -s -m command -a 'rm -rf /tmp/etc' -->remove a directory with a command module


Execution commnads:
------------------
 ansible-playbook apache.yml

 ansible-playbook -v apache.yml
 ansible-playbook -vv apache.yml
 ansible-playbook -vvv apache.yml

 ansible-playbook -b -i common-hosts tomcat-java-jenkins.yml
 ansible-playbook -b app -i common-hosts tomcat-java-jenkins.yml


 
 Ansible 
 
 
 nano -w /etc/ansible/hosts
 
 wildcads . hosts/groups   subscirpts   exclusion  intersection regex  


Inventory
static inventory
contains a list of human readable hostname/ip pairs
list of nodes you wish to interact with
INI format
can contain groups [groupname] web1 ansible_ssh_host=10.10.10.10
dynamic
static inventories have limitations in a cloud environment (nodes are coming and going, etc.)
can be a script as opposed to an ini

 
 

###
#  Add servers
###

[centos]
192.168.1.20          ansible_user=root    ansible_port=3333    ansible_ssh_pass=48f9UnvZ8s
192.168.3.200         ansible_user=root    ansible_port=3333    ansible_ssh_pass=1FkN02oMZnb0
 
 
[ubuntu]
10.0.10.20           ansible_user=root       ansible_ssh_pass=6TgUXt3ZaLok
10.0.11.20         ansible_user=root       ansible_ssh_pass=GAHjsJt63RlW


[appserver]
10.0.10.20           ansible_user=root       ansible_ssh_pass=6TgUXt3ZaLok
10.0.11.21           ansible_user=root       ansible_ssh_pass=48f9UnvZ8s


###
#  Check connect
###

ansible proxyserver -m command -a "uname -a"

###
#  Get info aboute os type
###

ansible proxyserver -m setup -a "filter=ansible_distribution*"
 

##############################
Task definitions 
##############################



[root@master lab04]# cat ansible.cfg
[defaults]
remote_user = root
inventory   = environments/prod
retry_files_save_path = /tmp
host_key_checking = False
log_path=~/ansible.log
[root@master lab04]#

root@master environments]# cat prod
[local]
localhost  ansible_connection=local


[web]
node1.rmohan.com
node2.rmohan.com

[app]
node3.rmohan.com

[db]
demo1.rmohan.com
demo2.rmohan.com


ansible all -a hostname

ansible all -a uptime

ansible app -a free 

ansible '*' -m ping 


particular  groups 

ansible app:db -a "uptime"

exclude the excluede particular group 

ansible app:!db -a "uptime"

ansible all -a "uptime"   --limit app 

[root@master lab04]# ansible all -a "uptime"   --limit app
node3.rmohan.com | SUCCESS | rc=0 >>
 12:23:31 up  2:33,  2 users,  load average: 0.00, 0.01, 0.05
 
ansible -m ping hosts --private-key=~/.ssh/keys/id_rsa -u centos

check the modules on the server 

 
ansible-doc -l | wc -l



ansible app -a "yum install -y docker-engine"

ansible all -f 1 -a "free" 

ansible app -s -m group -a "name=admin state=present"

ansible app -s -m user -a "name=devops group=admin createhome=yes"

ansible app -m copy -a "src=/root/anaconda-ks.cfg dest=/tmp/anaconda-ks.cfg"



Limit to one or more hosts
This is required when one wants to run a playbook against a host group, but only against one or more members of that group.

Limit to one host

ansible-playbook playbooks/PLAYBOOK_NAME.yml --limit "host1"
Limit to multiple hosts

ansible-playbook playbooks/PLAYBOOK_NAME.yml --limit "host1,host2"
Negated limit. NOTE: Single quotes MUST be used to prevent bash interpolation.

ansible-playbook playbooks/PLAYBOOK_NAME.yml --limit 'all:!host1'

Limit to host group

ansible-playbook playbooks/PLAYBOOK_NAME.yml --limit 'group1'

Limiting Tasks with Tags
Limit to all tags matching install

ansible-playbook playbooks/PLAYBOOK_NAME.yml --tags 'install'

Skip any tag matching sudoers

ansible-playbook playbooks/PLAYBOOK_NAME.yml --skip-tags 'sudoers'

Troubleshooting Ansible

http://docs.ansible.com/ansible/playbooks_checkmode.html

Busted Cache
Sometimes Ansible has a tendency to hold on to variables too long, which causes Ansible to think that a task/operation had already been done or changed when in fact it didn't.

A simple fix is to flush the redis cache during a code execution.

This can be done like this:

ansible-playbook playbooks/PLAYBOOK_NAME.yml --flush-cache

Check for bad syntax
One can check to see if code contains any syntax errors by running the playbook.

Check for bad syntax:

ansible-playbook playbooks/PLAYBOOK_NAME.yml --syntax-check


Running a playbook in dry-run mode
Sometimes it can be useful to see what Ansible might do, but without actually changing anything.

One can run in dry-run mode like this:

ansible-playbook playbooks/PLAYBOOK_NAME.yml --check

When all else fails


  
 

STEP BY STEP EXECUTION

  ansible-playbook systems.yml --step




Modules
Sometimes Ansible just can't cut performing a task using the built-in modules. Raw module to the rescue!

Using raw module to run command similar to running directly via SSH:

ansible -m raw -s -a "yum install libselinux-python -y" new-atmo-images


ansible app -m yum -b -a "name=vim state=installed"


 ansible web -b -m group -a "name=admin gid=98 state=present"

ansible 'web:!db' -b -m user -b -a "name=abc state=present uid=7001  group=admin"


ansible app -m command -a "mkdir /tmp/test creates=/tmp/test"





Targeting Hosts
all or all/*
Groups
Exclusion (webservers:!nginxservers)
Intersection (webservers:&staging)
Other
combos
hostname/ip
regex
--limit


Executing a Task
ad-hoc commands are single tasks which leverage modules
modules are code executed on the remote host
host machine needs python
ansible ships the module over via ssh and executes them on the host
ansible core modules
return json data
written in python ^^

ansible web -f 10 -m setup (set fork level to 10)


useful for setting fork level to one to do a rolling deploy - finish one node before moving to the next

Tasks
ansible is idempotent - will not blindly perform the same task twice - knows that it doesn't have to redo work
notice the "changed" key in the output
Copy ansible hosts -m copy -a "src=/path/to/src dest=/path/to/dest"
Install nginx and add a user ansible all -m apt -a "pkg=nginx state=present" (must target debian/ubuntu) ansible all -m yum -a "pkg=nginx state=present" (centos) ansible all -m package -a "pkg=nginx state=present" (generic) - has to do fact gathering first ansible all -m user -a "name=bob state=present"
clone a git repo ansible appservers -m git -a "repo=https://github.com/user/repo dest=/home/user/repo"
have to setup the authentication creds on the host machine
service status ansible -m service -a "name=httpd state=started"
Backgrounding tasks, with opt polling ansible all -B 3600 -a "/usr/bin/long_running_op --do-stuff" ansible all -B 1800 -P 60 -a "/usr/bin/long_running_op --do-stuff"
setup gathers facts ansible host -m setup




modules 

raw
command
shell
script
expect 
win_command 



Parallelism and Shell Commands

To run reboot for all your company servers in a group, 'abc', in 12 parallel forks −
$ ansible app -a "/sbin/reboot" -f 12
By default, Ansible will run the above Ad-hoc commands form current user account. If you want to change this behavior, you will have to pass the username in Ad-hoc commands as follows −
$ ansible  abc -a "/sbin/reboot" -f 12 -u username






Plays

An ordered set of tasks executed on a selection of hosts with controls over how the tasks operate hosts is required
can specify vars and use them in your tasks

Playbooks
An ordered set of plays with an inventory, global variables, forks, inventory limits, etc.



This is where Ansible comes into play; Ansible introduced a concept called playbooks, which is a simple text ﬁle that describes conﬁguration, a policy or a set of steps which can be used to conﬁgure a remote server. Playbooks are very powerful and very simple; they are expressed in YAML format, which is human-readable data serialization format 1.
Playbooks are the files where Ansible code is written. Playbooks are written in YAML format. YAML stands for Yet Another Markup Language. 
Playbooks are one of the core features of Ansible and tell Ansible what to execute. They are like a to-do list for Ansible that contains a list of tasks.
Playbooks contain the steps which the user wants to execute on a particular machine. 
Playbooks are run sequentially. Playbooks are the building blocks for all the use cases of Ansible.


# This playbook play all post common installation for all servers
- hosts: all sudo: True
tasks: - name : Update yum cache yum : update_cache=yes
- name : yum upgrade yum : upgrade=yes
- name : yum dist yum : upgrade=dist
- name: Change hostname hostname: name=web01
- name: install ntp yum: pkg=ntp state=present





The Different YAML Tags
Let us now go through the different YAML tags. The different tags are described below −

name
This tag specifies the name of the Ansible playbook. As in what this playbook will be doing. Any logical name can be given to the playbook.

hosts
This tag specifies the lists of hosts or host group against which we want to run the task. 
The hosts field/tag is mandatory. It tells Ansible on which hosts to run the listed tasks. 
The tasks can be run on the same machine or on a remote machine. One can run the tasks on multiple machines and hence hosts tag can have a group of hosts’ entry as well.

vars
Vars tag lets you define the variables which you can use in your playbook. Usage is similar to variables in any programming language.

tasks
All playbooks should contain tasks or a list of tasks to be executed. 
Tasks are a list of actions one needs to perform. A tasks field contains the name of the task. 
This works as the help text for the user. It is not mandatory but proves useful in debugging the playbook. 
Each task internally links to a piece of code called a module. A module that should be executed, and arguments that are required for the module you want to execute.










Control Structures
with_item
- name: add users
  user: name={{ item }} state=present groups=wheel
  with_items:
    - user1
    - user2
Or...

- name: add users
  user: name={{ item }} state=present groups=wheel
  with_items: "{{ my_items_var }}"

  
  
  Conditionals
Task conditionals
use when
- name: install nginx
  yum: name=nginx state=present
  when: ansible_distribution == 'CentOS'
- name: install nginx
  apt: name=nginx state=present
  # bare variables complaints?
  when: ansible_distribution == 'Debian'
group_by: create dynamic groups
- name: grouping play
  hosts: all
  tasks:
    # this will create groups CentOS, Ubuntu, etc.
    - name: create distro groups
      group_by: key={{ ansible_distribution }}

- name: CentOS play
  hosts: CentOS
  gather_facts: False
  tasks:
    - # only affect CentOS hosts
Status Control
failed_when
changed_when
Local Action
local_action - perform task on local machine
connection: local - perform this play locally



Handlers
Like regular tasks - but only ran in the task contains a notify directive
Ex: If the nginx conf is changed, we probably want to restart nginx
Don't need to do this all the time, only when it changes
refer to handlers by name
# at the bottom of a play
handlers:
  - include: handlers/handlers.yml
  
  
  
  
Templates
use jinja templates to craft files and send them to server
- template: src=/mytemplates/foo.j2 dest=/etc/file.conf owner=bin group=wheel mode=0644





---
- name: groupadd ansible
  group:
    gid=200
    name=ansible
    state=present

- name: useradd ansible
  user:
    uid=200
    name=ansible
    group=ansible
    groups=admin
    shell=/bin/bash

- name: mkdir .ssh
  file:
    path=~ansible/.ssh
    owner=ansible
    group=ansible
    mode=0700
    state=directory

- name: copy public key
  copy:
    src=files/authorized_keys
    dest=~ansible/.ssh/authorized_keys
    owner=ansible
    group=ansible
    mode=0600

- name: check ansible in sudoers
  shell: cat /etc/sudoers | grep ansible
  register: sudoers_check
  ignore_errors: yes

- name: add ansible to sudoers
  shell: echo "%ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
  when: sudoers_check|failed
  
  
  Loops
  
  For this purpose you can use loops. A loop is a sequence of statements which is speciﬁed once but which may be carried out several times in succession2. . Following is a standard loop, which you can specify a list with with_items directive and give the variable reference item to the pkg argument as parameter. This code is equivalent as one above.
- name: Install nginx and php-fpm apt: pkg="{{item}}" state=present with_items: - nginx - php5-fpm


Specifying list of items is also possible with traditional array structure :
- name: Install nginx and php-fpm apt: pkg="{{item}}" state=present with_items: ["nginx","php5-fpm"]

Because our web application will have some ﬁles and we have to copy them to the server, for this purpose we can use loops for copying ﬁles as well:
- name: Copy files copy: src="{{item}}" dest="/usr/share/nginx/www/{{item}}" with_items: - index.html - myapp.php




Handlers
Now we know how to install nginx, copy ﬁles and make the conﬁguration ﬁles based on variables and templates. 
But we also need to restart the server because we do change the conﬁguration of the server. 

Despite there are many way t do this kind of actions,bes tway to go is to use some thing easy and simple: handlers. 
In ansible handlers arej ust list of  tasks, like others, difference is that for a task to be executed handler have to be notiﬁed. 
Let’s give an example for this: following handler will restart the nginx when it is being notiﬁed by a task. 
In the example below when template module completes it’s job it notiﬁes the handler. The notify directive contains the name of the handler.

tasks: - name: write nginx.conf template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf notify: - restart nginx
handlers: - name: restart nginx service: name=nginx state=restarted enabled=yes
The notify directive registers an change event and informs the handlers. If there are no changes there are no triggers. In the example above, service module, controls daemon on remote hosts. The parameter state in this case is set to restart, meaning it will always restart the nginx. The parameter enabled means service is enabled on boot. The beauty of handlers is that they could be notify n time, but they will be executed only ones after all tasks are completed in a play.





ansible-galaxy init --init-path roles/ apache

ansible-galaxy --help



