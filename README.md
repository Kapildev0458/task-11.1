# task-11.1
# first create this code in task11.yml file.
- hosts: all
  vars: 
    - hdfs_dir: /master
    - node: name
    - ip_addr: 0.0.0.0
    - port: 9001

  tasks:
    - name: make directory for master node
      file:
        state: directory
        path: "/master
        
    - name: Copying JDK
      copy: 
        src: "/jdk-8u171-linux-x64.rpm"
        dest: /root

    - name: Copying Hadoop
      copy: 
        src: "/hadoop-1.2.1-1.x86_64.rpm"
        dest: /root

    - name: Installing JDK
      yum:
        name: "/jdk-8u171-linux-x64.rpm"
        state: present

    - name: Installing Hadoop
      command: "rpm -i /hadoop-1.2.1-1.x86_64.rpm --force"

    - name: Configuring hdfs-site
      template:
        src: hdfs-site.xml
        dest: /etc/hadoop/hdfs-site.xml
        
    - name: Configuring core-site 
      template:
        src: core-site.xml
        dest: /etc/hadoop/core-site.xml
    
    - name: Formatting the Directory
      command: hadoop namenode -format

    - name: Starting the Namenode Server
      command: hadoop-daemon.sh start namenode 
  
    - name: Checking Status
      command: jps

    - name: Checking Report
      command: hadoop dfsadmin -report
      
      
#hdfs-site.aml file

<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration>
<property>
<name>dfs.{{ node }}.dir</name>
<value>{{ hdfs_dir }}</value>
</property>
</configuration>


# core-site.xml file
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration>
<property>
<name>fs.default.name</name>
<value>hdfs://{{ ip_addr }}:{{ port }}</value>
</property>
</configuration>

#to run this playbook use command : ansible-playbook task11.yml
