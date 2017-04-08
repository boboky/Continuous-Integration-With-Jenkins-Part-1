#  Continuous integration with Jenkins [ April 2017]

This guide provides simple practical steps to learning and using Jenkins. It will take about an hour to complete

---

## 

You will try out different excercises using Jenkins to fulfil Continous Integration process

---

## Intended Audience

Users with basic understanding of Linux, Git, Continuous Integration.

---

## Requirements

Ensure the following is installed and working

- putty https://www.dropbox.com/sh/cho8pwn3tlfz37o/AADcF15c1vXwVhu4LE23BkVIa?dl=0 
- create a Github account [https://github.com/] and ensure you are able to login
- create 2 AWS EC2 Instances and ensure you are able to connect to each one of them via Putty

**DO NOT PROCEED UNTIL THESE ARE PRESENT**

---

## Intro

- Jenkins is a free source that can handle any kind of build or continuous integration. Jenkins works with a number of technologies mainly through plugins
- Continuous Integration is a development practice that requires developers to integrate code into a shared repository at regular intervals. This concept was meant to remove the problem of finding later occurrence of issues in the build lifecycle
- Developers checkin their codes -> Jenkins build job is triggered to compile, build and test -> build output is available Jenkins dashboard.


---

###  Task 1: Prepare your Linux server for Jenkins Install

- Connect to AWS Serve 1
- Update yum, install Jenkins and Java (OpenJDk 1.8.0)
- start Jenkins 
- Connect to Jenkins 

#### Update yum, install Jenkins and Java (OpenJDk 1.8.0)

    sudo yum update -y
    sudo yum install wget -y
    sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
    sudo yum install jenkins
    sudo yum install java-1.8.0-openjdk-devel


#### start Jenkins

    sudo service jenkins start


#### Connect to Jenkins  

    service iptables stop [ensure iptables is stopped. ]
    ensure access is given on port 8080 in AWS InBound SecurityGroup
    connect to http://publicip/8080/jenkins


#### Create an Account in Jenkins

    First unlock Jenkins...
    Go to http://publicIp/jenkins
    cat /var/lib/jenkins/secrets/initialAdminPassword
    copy the value into 'password' textarea and continue
    Goto to http://publicip:8080



### Task 2. Working with Jenkins.

- Confirm the version of Jenkins installed
- update admin user password
- Create another user account, call it [admin1]
- Create a Jenkins Item ( a job) to copy file /tmp/hello to /tmp/deploy directory
- run the job (check the job console to view the log)
- run the job for the second time. If the break, fix.


#### Create a Jenkins Item ( a job) to copy file /tmp/hello to /tmp/deploy directory

    echo "testing jenkins" >> /tmp/hello.txt
    Go to Jenkins->NewItem 
    ["Enter an item name" - HelloWorld] -> [Freestyle Project] -> OK
    ["Build] -> [Add a build step] -> [Execute shell] 
    `mkdir /tmp/jenkins`
    `cp /tmp/hello.txt /tmp/jenkins`
    ["APPLY] -> [SAVE]
    ["Build Now]. Now view build console 


### Jenkins to do [Try it  yourself]
- Update Jenkins to update you via email after evey successful execution 
- Update Jenkins configuration to run over HTTPS




### Task 3. Git Install.

- install git on the server and confirm version
- configure name and email

#### Install git on the server

    sudo yum install git -y
    get the version of git [ git --version ]

#### configure name and email

    git config --global user.name "Olu Mike"
    git config --global user.email "shegoj@yahoo.com"

### Task 3. Fork APP into your Github space.

- Fork JavaWeb-App repo from https://github.com/shegoj/javawebapp  into your github



### Task 4. Integrate Git with Github Account.

- Download or clone javawebapp from your github space unto the Jenkins server
- create ssh private/public key on your Linux Server
- copy public key into your github account 

#### Download of clone javawebapp from your github space

    create /workapp directory
    cd to /workapp directory
    git clone git@github.com:youraccount/javawebapp.git   [ this will fail, next steps will fix]

#### create ssh private/public key on your Linux Server

    ssh-keygen [ select default values] This will create a private key [ ~/.ssh/id_rsa ] and a public key [ ~/.ssh/id_rsa.pub ]

#### copy public key into your github account 

    go to your github account. got to Settings -> SSH And GPG Keys -> New SSH key
    Provide a title [Jenkins]  and copy content of ~/.ssh/id_rsa.pub into 'Key'
    copy content of ~/.ssh/id_rsa.pub into 'Key' [ cat  ~/.ssh/id_rsa.pub]
    Now run git clone git@github.com:youraccount/javawebapp.git  [ this should now work]




## Summary

We have looked at how to install and configure Jenkins and Git on a server, create a job item and troubleshoot.
