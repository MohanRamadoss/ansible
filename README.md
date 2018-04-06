##### Steps for deployment of Ansible on CentOS 7

##### Dependency Tasks

### Install EPEL
    sudo yum install epel-release

### Install pending updates
    sudo yum -y update

##### Install Ansible

### Install Ansible
    sudo yum -y install ansible

### Verify the Version
     ansible --version

	
	

[db]
node1.rmohan.com
[app]
node2.rmohan.com
[db]
node3.rmohan.com


ansible all --list-hosts
ansible db --list-hosts 
ansible db -m ping 


connect with password authentication, it's possible to do with "k"
ansible db -k -m command -a "uptime" 

ansible db -k -m command -a "cat /etc/shadow" -b --ask-become-pass 
 
other user's priviledge except root, specify the option "--become-user=xxx".
If you'd like to use another way to use priviledge except sudo (su | pbrun | pfexec | runas), specify the option "--become-method=xxx". 


ansible db -m ping
ansible db -m command -a uptime
ansible db -a "tail /var/log/dmesg"


ansible -m ping db
ansible -m ping -all
ansible -m command -a "df -h" db
ansible -m command -a "free -mt" db
ansible -m command -a "uptime" all
ansible -m command -a "arch" all
ansible -m shell -a "hostname" all
ansible -m command -a "df -h" db > /tmp/df_outpur.txt

ansible all -a "echo hello world"
ansible all  -m ping
ansible db -m ping
ansible db -m setup -l node-1
ansible db -m command -a "hostname"
ansible db -m command -a "hostname" -o
ansible db -m command -a "uptime"
ansible db -m shell -a 'echo $TERM'
ansible db -b -m yum -a "name=httpd state=present"

ansible web -b -m service -a "name=httpd state=started"
ansible web -b -m service -a "name=httpd state=stopped"

ansible web -a "/sbin/reboot" -f 10



Adhoc Commands

ansible web -a "yum update -y"
ansible app -a "yum -y install tomcat"
ansible app -a "service tomcat status"
ansible app -a "service tomcat start"
ansible app -a "yum -y install curl wget"
ansible app -a "curl web"
ansible app -a "bash -c 'curl -k https://github.com/opstree-ansible/ansible-training/blob/master/attendees/exercise/application/sample.war > /var/lib/tomcat/webapps/sample.war'"
ansible app -a "service tomcat restart"
ansible app -a "curl node2.rmohan.com:8080/sample/"


ansible centos -m copy -a "src=test.txt
ansible centos -m copy -a "src=test.txt dest=/tmp/test.txt"
ansible centos -m yum -a "install libselinux-python"
ansible centos -m copy -a "src=test.txt dest=/tmp/test.txt"



 vi playbook_sample.yml
# target hostname or group name
- hosts: web
# define tasks
  tasks:
# task name (any name you like)
  - name: Test file
# use file module to set the file state
    file: path=/tmp/test.conf state=touch owner=root group=root mode=0600


run Playbook
ansible-playbook playbook_sample.yml 

ansible web1 -m command -a "ls -l /tmp/" 

[root@controller test]# ansible web -m command -a "ls -l /tmp/test.conf"
node1.rmohan.com | SUCCESS | rc=0 >>
-rw------- 1 root root 0 Mar 31 20:29 /tmp/test.conf




create a Playbook which Apache httpd is installed and running.
vi playbook_sample2.yml
- hosts: web
# use priviledge (default : root)
  become: yes
# the way to use priviledge
  become_method: sudo
# define tasks
  tasks:
  - name: httpd is installed
    yum: name=httpd state=installed
  - name: httpd is running and enabled
    service: name=httpd state=started enabled=yes


ansible-playbook -v playbook_sample2.yml --ask-become-pass

ansible web -m shell -a "/bin/systemctl status httpd | head -3" -b --ask-become-pass

[root@controller test]# ansible web -m shell -a "/bin/systemctl status httpd | head -3" -b --ask-become-pass
SUDO password:
node1.rmohan.com | SUCCESS | rc=0 >>
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2018-03-31 20:36:52 +08; 46s ago




/root/ansible/playbook/test

playbook_sample.yml

- hosts: db
  become: yes
  become_method: sudo
  tasks:
  - name: General packages are installed
    yum: name={{ item }} state=installed
    with_items:
      - vim-enhanced
      - wget
      - unzip
    tags: General_Packages
	

[root@controller test]# ansible-playbook playbook_sample.yml --ask-become-pass
SUDO password:

PLAY [db] *****************************************************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************************************************************************************************************************
ok: [node3.rmohan.com]

TASK [General packages are installed] *************************************************************************************************************************************************************************************************************************************
ok: [node3.rmohan.com] => (item=[u'vim-enhanced', u'wget', u'unzip'])

PLAY RECAP ****************************************************************************************************************************************************************************************************************************************************************
node3.rmohan.com           : ok=2    changed=0    unreachable=0    failed=0

[root@controller test]#


ansible db  -m shell -a "rpm -qa | grep -E 'vim-enhanced|wget|unzip'" --ask-become-pass 






variables from "GATHERING FACTS"
 vi playbook_sample3.yml
 
# refer to "ansible_distribution", "ansible_distribution_version"
- hosts: target_servers
  tasks:
  - name: Refer to Gathering Facts
    command: echo "{{ ansible_distribution }} {{ ansible_distribution_version }}"
    register: dist
  - debug: msg="{{ dist.stdout }}"


[root@controller test]# ansible-playbook playbook_sample3.yml

PLAY [web] ****************************************************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************************************************************************************************************************
ok: [node1.rmohan.com]

TASK [Refer to Gathering Facts] *******************************************************************************************************************************************************************************************************************************************
changed: [node1.rmohan.com]

TASK [debug] **************************************************************************************************************************************************************************************************************************************************************
ok: [node1.rmohan.com] => {
    "msg": "CentOS 7.4.1708"
}

PLAY RECAP ****************************************************************************************************************************************************************************************************************************************************************
node1.rmohan.com           : ok=3    changed=1    unreachable=0    failed=0



 vi playbook_sample4.yml
 - hosts: target_servers
  become: yes
  become_method: sudo
  handlers:
  - name: restart sshd
    service: name=sshd state=restarted
  tasks:
  - name: edit sshd_config
    lineinfile: >
      dest=/etc/ssh/sshd_config
      regexp="{{ item.regexp }}"
      line="{{ item.line }}"
    with_items:
    - { regexp: '^#PermitRootLogin', line: 'PermitRootLogin no' }
    notify: restart sshd
    tags: Edit_sshd_config
	

ansible-playbook playbook_sample4.yml --ask-become-pass 



export JAVA_HOME=/opt/java/java/
export JRE_HOME=/opt/java/java/jre
export PATH=$PATH:/opt/java/java/bin:/opt/java/java/jre/bin



