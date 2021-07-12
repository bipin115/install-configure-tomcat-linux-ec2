Install Tomcat On Linux/Unix Box & Configure with Jenkins

Step 1: Install Java

sudo apt-get update
sudo apt-get install default-jdk


Step 2: Install Tomcat

sudo apt install unzip wget
cd /tmp
wget http://mirrors.estointernet.in/apache/tomcat/tomcat-9/v9.0.48/bin/apache-tomcat-9.0.48.zip
unzip apache-tomcat-*.zip
sudo mkdir -p /opt/tomcat
sudo mv apache-tomcat-9.0.48 /opt/tomcat/


Step 3: Change Tomcat Server Port

Jenkins is running on Port 8080, and tomcat defalut port is also 8080. So we need to change the Tomcat port, I am changing it to 9090

Go to /opt/tomcat/apache-tomcat-9.0.48/conf/server.xml file

Search for Connector and change the Port Value, save the file.


Step 4: Change Permission of Scripts in /bin

cd /opt/tomcat/apache-tomcat-9.0.48/bin
ls -la
sudo chmod +x *


Step 5: Start tomcat server and accss via browser on port 9090

/opt/tomcat/apache-tomcat-9.0.48/bin/startup.sh



Step 6: Configure Jenkins with Tomcat for Auto Deployment of Artifacts.

Set Credentials of Tomcat that Jenkins use.

cd /opt/tomcat/apache-tomcat-9.0.48/conf

update tomcat-users.xml file.

If nothing works simply create another user in tomcat-users.xml file with magnager-script role assigned and set this user credential to jenkins .

In tomcat-users.xml file

<tomcat-users>
<user  username="deployuser" password="deployuser" roles="manager-script" />
<user username="admin" password="admin" roles="manager-gui" />
</tomcat-users>


Step 6 : Restart the tomcat server

/opt/tomcat/apache-tomcat-9.0.48/bin/shutdown.sh
/opt/tomcat/apache-tomcat-9.0.48/bin/startup.sh


ISSUE TOMCAT Jenkins : 

This seems to be a Jenkins bug but I got around the problem by setting up following configuration in Tomcat:

Edit the file /webapps/manager/META-INF/context.xml:

Previous:

<Context antiResourceLocking="false" privileged="true">
  <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
</Context>

Change this file to comment the Value:

<Context antiResourceLocking="false" privileged="true">
  <!--
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
    -->
</Context>
