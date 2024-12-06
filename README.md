registration-app
<br>
Test33
# Devops
Devops Project
This is a DevOps project which consists of integrating Jenkins,maven build,apache.

Step1:Create an EC2 amazon-linux instance
Go to Launch Instances>create an EC2 instance with t2-micro instance type.

Step2:Install Jenkins into the EC2 instance
Steps are:
1.First we will need to be a root user - sudo su > cd .. > only the public Ip in home directory.
2.Script to install jenkins:https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/

sudo yum update -y

sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

sudo yum upgrade

sudo dnf install java-17-amazon-corretto -y

sudo yum install jenkins -y

sudo systemctl enable jenkins

sudo systemctl start jenkins

sudo systemctl status jenkins: checks out the status pof the service

3)Now need to enable port 8080 on SG - inbound rules as Jenkins connects on port 8080.

4)Take the public Ip address of the instance > copy paste in url along with port :8080 
Jenkins works!!!!!

Now inside Jenkins > we will need to cat "file path" and we will get the administrator key and login.

5)Inside jenkins need to install the suggested plugins:
The heart of Jenkins lies in plugins as plugins are the most important for the functioning of Jenkins.
In case we will want to chnage the hostname: hostname JENKINS-SERVER > cd etc/ > type in JENKINS-SERVER.

Create first username - admin pwd: admin@123

Start using JENKINS!!!

INSTALL AND CONFIGURE THE MAVEN:
1.Go to Ec2 instance of jenkins > go to maven.apache.org/downloads > copy the tar gz file path file > cd /optwget paste the tar path file (do this through root user) > ls(we will see the tar file which has been downloaded).

Now we will extract this file through tar -xzvf apache.maven.....> ls
mv apache.maven..... maven - moved folder to maven 
cd maven/ > ls
cd bin/
./mvn -v > it is shwoing the mavcen version > but if we go outisde bin folder and run the command it doesnot show the output,so in order to do that we will need to set the env variables.
First go to root home directory > ll -a ( shows all files) > we will have to edit the file - .bash_profile > in order to find the java path > duplicate the session > type : find / -name java-17* > this will show the path of java file > we will have to set this inside JAVA_HOME(.bash_profile) > save and exit
echo $PATH (doesnt work) type: source .bash_profile > echo $PATH
Now we will be able to run maven from anywhere on the server : mvn -v

INSTALL MAVEN PLUGIN AND CONFIGURE JENKINS FOR MAVEN:
Go to Jenkins> install plugin:maven > maven integration > go to manage jenkins > tools > under jdk - click add jdk , name:java17, javahomepath : we will need to copy the path in find / -name java-17*.
Maven Installation > add Maven > name : maven , MAVNE_HOME: /opt/maven > apply & save
Avaialble plugin > github > turn off Github Barnch Source Plugin and turn on the other two > restart 

Now in EC2 instance we will need to isntall git :
yum install git 
login to jenkins
Go to maven project > give the git url repo link > scm : git , credentials : none > branches: main, inside build: goals and option : clean installl> apply & save
build now > jenkins willl build the repo > go to workspace > tagert > webapp > we will be able to see webapp.war --- build file of the process .

SETUP TOMCAT SERVER:
1.Setup the EC2 server 
2.Go to SG > enable port 8080
3.Install Java
4.Go to  tomcat.apache.org/downloads > copy the tar file > go to ec2>cd /opt wget paste the tar file(root user) >ls >extract the file: tar -xzvf paste the apache-tomcat......> ls > mv  apache-tomcat...... tomcat > cd tomcat/> cd bin/ > vim ./startup.sh > tomcat got started in server
copy public ip of server > url : 8080
Tomcat works!!!!!

cd .. > we will be isnide tomcat directory > find / -name context.xml > two files need to edit >vim inside path of context.yml > #need to comment those lines > vim insiode path of second yml file > #need to comment > cd bin/ > ./shutdown.sh > ./startup.sh > create credentails in tomcat server 
go to terminal ec2 > cd ..>cd conf/> go to vim tomact-users.xml > inside file type in 3 new user details > cd ..>cd bin/ >  ./shutdown.sh > ./startup.sh > type oin username and password > tomcat inside configuration works!!!

INSTALL TOMCAT SERVER WITH JENKINS
1.Go to jenmins > available plugins > deploy to container > install > go to security > global credentials > add credentials > username and password > username:deployer password:---- > added 
Go to Job > new job >maven project > go to configuration > scm: git , url of github , build:pom.xml ,goals and options:clean install > build options:deploy war to container > WAR/EAR:**/*.war , add container:tomcat 8* , credentails : to,cat credenatils created previously in global , give url of tomcat server form tomcat page > apply & save
We will be able to see tomcat server button works !!!!

AUTOMATE BUILD AND DEPLOY TO JENKISN USING POLLSCMAND VERIFY CI/CD:
1.inside configuration> poll SCM > ***** > apply & save 
Go to repo > pipeline > src main weba-pp > index.jsp > make any change s> commit > go to jenkins job> within a min it triggers the pipeline > build job and deploy job is running !!
Browse the tomcat server ip address:8080 
TOMCAT WORKS!!!!!

If I need to make a gap within thank you , will edit the code in github and build is triggered and on refreshing the URL we are able to see the difference.

