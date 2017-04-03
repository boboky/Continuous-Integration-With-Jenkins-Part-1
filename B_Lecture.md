
## Day 2

- Install tools for Managing Java App


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
    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.121-0.b13.el6_8.x86_64 ## confirm if directory exist


    source /etc/profile.d/maven.sh

#### Confgure Maven with Java
    mvn -version


    
#### start Jenkins

    service jenkins start


#### Connect to Jenkins  

    service iptables stop [ensure iptables is stopped. ]
    ensure access is given on port 8080 in AWS InBound SecurityGroup
    connect to http://publicip/8080/jenkins


#### Create an Account in Jenkins

    First unlock Jenkins...
    Go to http://publicIp/jenkins
    cat /var/lib/jenkins/secrets/initialAdminPassword
    copy the value into 'password' textarea and continue
    Goto to http://localhost:8080



### Task 2. Working with Jenkins.

- Confirm the version of Jenkins installed
- Create a user account, call it [admin1]


## Continous Integration without Jenkins


### Task 3. Git Install.

- install git on the server and confirm version
- configure name and email

#### Install git on the server

    sudo yum install git
    get the version of git [ git --version ]

#### configure name and email

    git config --global user.name "Olu Mike"
    git config --global user.email "shegoj@yahoo.com"

### Task 3. Fork APp into your Github space.

- Fork XXX-App from xxxxxxxxxxxxxxxxx to into your github



### Task 4. Integrate Git with Github Account.

- Download of clone XXX-App from your github space
- create ssh private/public key on your Linux Server
- copy public key into your github account 

#### Download of clone XXX-App from your github space

    create /workapp directory
    cd to /workapp directory
    git clone git@github.com:youraccountname/XXX-App.git  [ this will fail, next steps will fix]

#### create ssh private/public key on your Linux Server

    ssh-keygen [ select default values] This will create a private key [ ~/.ssh/id_rsa ] and a public key [ ~/.ssh/id_rsa.pub ]

#### copy public key into your github account 

    go to your github account. got to Settings -> SSH And GPG Keys -> New SSH key
    Provide a title [Jenkins]  and copy content of ~/.ssh/id_rsa.pub into 'Key'
    copy content of ~/.ssh/id_rsa.pub into 'Key' [ cat  ~/.ssh/id_rsa.pub]
    Now run git clone git@github.com:youraccountname/XXX-App.git  [ this should now work]




## Summary

We have looked at Linux basic commands, directory and files manipulation.For more challenging practical over what we have covered today, check here

Here is Linux timeline representation of the distros.. ![https://github.com/shegoj/Linux_Essentials/blob/master/image1.svg](https://github.com/shegoj/Linux_Essentials/blob/master/image1.svg)
