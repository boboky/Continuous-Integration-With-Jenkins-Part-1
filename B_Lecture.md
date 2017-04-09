
## Day 2

- Install tools for Managing Java App ( Manually... without Jenkins)


---

###  Task 1: Install Maven 

- Install maven on the server
- Confgure Maven with Java
- Get maven version


#### Install maven on the server

    cd /opt
    sudo wget http://www-eu.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz 
    sudo tar xzf apache-maven-3.3.9-bin.tar.gz
    sudo ln -s apache-maven-3.3.9 maven 

#### Confgure Maven with Java

    sudo vi /etc/profile.d/maven.sh
    add the following 
    =================
    export M2_HOME=/opt/maven
    export PATH=${M2_HOME}/bin:${PATH}
    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.121-0.b13.el7_3.x86_64 ## confirm if directory exist


    source /etc/profile.d/maven.sh

#### Confgure Maven with Java
    mvn -version



###  Task 2: Compile and Test JavaWeb App 

- Use Maven to "build" the Java code
- Use Maven to "Test" the Java code
- Use Maven to package the Java code

    
#### Use Maven to "build" the Java code

    cd to ~/workdir/javawebapp/CounterWebApp  . 
    mvn clean install  


#### Use Maven to "Test" the Java code  

    mvn tomcat:run
    Note:ensure access is given on port 8080 in AWS InBound SecurityGroup, iptables service is stop and  Jenkin process is not running
    connect to http://publicip:8080/CounterWebApp/mkyong and test accordingly


#### Use Maven to package the Java code

    stop running process  with crlt+c
    mvn package  . This generates a war file. Note the location of the file. This is the "finished" file that will be deployed on the "production" server box. /home/ec2-user/workdir/javawebapp/CounterWebApp/target/CounterWebApp.war



###  Task 3: Prepare Produdction Server for Installation

- Install Java and Tomcat on the Server
- Test that Tomcat works on the Server
- Get maven version


#### Install Java and Tomcat on the Server

    sudo yum update
    sudo yum install java-1.8.0-openjdk-devel
    cd /tmp
    wget http://www.us.apache.org/dist/tomcat/tomcat-8/v8.5.13/bin/apache-tomcat-8.5.13.tar.gz
    tar xzf apache-tomcat-8.5.13.tar.gz 
    sudo mkdir -p /usr/local/apache2/tomcat8
    sudo mv apache-tomcat-8.5.13 /usr/local/apache2/tomcat8
    cd /usr/local/apache2/tomcat8/apache-tomcat-8.5.13/bin/

    

#### Test that Tomcat works on the Server

    sudo ./startup.sh 
    Note:ensure access is given on port 8080 in AWS InBound SecurityGroup, iptables service is stop and  Jenkin process is not running
    connect to http://publicip:8080 . Also try http://publicip:8080/CounterWebApp/mkyong


###  Task 4: Now Manually Deploy the JavaWebApp on the 'Production' server 

- Ensure you are able to copy files from Jenkin server to Prod server
- Copy war file from Jenkins to Prod
- Check if App is running

#### Ensure you are able to copy files from Jenkin server to Prod server
    From the Jenkins server ping prodserverIp. Ensure this works
    From the Jenkins server commandline type `ssh prodserver_publicIP`   to connect to Prod without using a password. This will most certainly fail. The next step should fix this.
    copy the content of ~/.ssh/ia_rsa.pub file on the Jenkins server on into ~/.ssh/authorized_keys on the Prod server. create the file if it doesn't exist. Make a backup of the file before you edit. ~/.ssh/authorized_keys file requires permission '600'
    From the Jenkins server commandline type `ssh prodserver_publicIP`   to connect to Prod without using a password. This should now work.
    From the Jenkins server commandline test if can copy file from the Jenkins server to Prod. create a file (hello.txt) then use `scp hello.txt prodserverip:/tmp` . Confirm if file is successfully copied]


#### Copy war file from Jenkins to Prod
    From the Jenkins server copy war file to Prod  
    scp /home/ec2-user/workdir/javawebapp/CounterWebApp/target/CounterWebApp.war prodserverip:/usr/local/apache2/tomcat8/apache-tomcat-8.5.13/webapps

#### Check if App is running
    http://publicip:8080/CounterWebApp/mkyong




## Summary. 
 
We have looked at installing and configuring maven, git and Jenkins. We have also worked with the tools. Now its time to automate the process via Jenkins. Try to configure Jenkins to run on port 80 (andd 443 i.e https) in readiness for Day 3. 
