#  Continuous integration with Jenkins [ April 2017]

We shall be leveraging Jenkins to automate our delivery pipeline

---

## 

You will try out different excercises using Jenkins to fulfil Continous Integration process

---

## Intended Audience

Participants of Day 1 and Day 2 Workshops.

---

## Requirements

Ensure the following is installed and working

- Ensure your Jenkins is not configured on port 8080
- Ensure ssh (passwordless) connections still works betweeen the Jenkins server and Prod 
- ensure git connection from the Jenkins server to Gthub still works

## So far

- Install and Configured required applications
- used git to download source code 
- compiled and build source code with maven
- ran test and packaged with maven
- deployed  war file to production via bash script
- tested that it works

---

## Intro

- So far we have used a manual means to compile, build, test and deploy the JavaWeb App. Now we shall be automating  build pipeline via Jenkins to achieve the same objective


---

###  Task 1: Create a Task to Download Source Code from GitHub

- Install 'GitHub plugin' Jenkins Plugin
- Configure the plugin
- Create a new task 'WebApp source Download' 
- Run task and fix to ensure it works

#### Install 'GitHub plugin' Jenkins Plugin

    sudo service jenkins start ## start jenkins and log in
    Goto Jenkins->Manage Jenkins ->Manage Plugins
    Goto Avaliable. Select 'GitHub plugin' ( Use can use filter find the plugin)
    select plugin and click 'Download now and install after restart'. This will kick it off
    select restart' checkbox to restart Jenkins


#### Configure the plugin

    Goto Jenkins->Manage Jenkins ->Global Tool Configuration
    Under GIT section, ensure "Path to Git executable" is 'git' and 'install automatically' is checked
    Click 'Apply'/'Save' button

#### Create a new task 'WebApp source Download'   

    Goto Jenkins->New Item->FreeStyleProject 'WebApp source Download'
    Under 'Source Code Management', select Git and  Repository URL pointing to https://github.com/youraccount/javawebapp
    Click 'Apply'/'Save' button


#### - Run task and fix to ensure it works

    Run the task and ensure it works. Check console to see what is happening behind the scene



### Task 2. Create Task to Build source with Maven.

- Install 'Maven Integration plugin' Jenkins Plugin
- Configure the plugin
- Create a new task 'WebApp build' 
- Run task and fix to ensure it works

#### Install 'GitHub plugin' Jenkins Plugin

    sudo service jenkins start ## start jenkins and log in
    Goto Jenkins->Manage Jenkins ->Manage Plugins
    Goto Avaliable. Select 'Maven Integration plugin' ( Use can use filter find the plugin) 
    select plugin and click 'Download now and install after restart'. This will kick it off
    select restart' checkbox to restart Jenkins


#### Configure the plugin

    Goto Jenkins->Manage Jenkins ->Global Tool Configuration
    Under Maven section, ensure 'install automatically' is checked, click 'Add Installer' Install from Apache Version:3.3.9	
    Click 'Apply'/'Save' button



#### Create a new task 'WebApp Build'   

    Goto Jenkins->New Item->Maven Project 'WebApp Build'
    Under 'Source Code Management', select Git and  Repository URL pointing to https://github.com/youraccount/javawebapp  CounterWebApp/pom.xml
    Under 'Build'
    Root POM' should be  CounterWebApp/pom.xml  , 'Goals and options' should be 'clean' ( without the quote). ## Please understand what we are doing here or ask questions if you don't
    Click 'Apply'/'Save' button


#### - Run task and fix to ensure it works

    Run the task and ensure it works. Check console to see what is happening behind the scene
    No change 'Goals and options' to 'clean install', the run task again



### Task 3. Add a Post-build step to the 'WebApp Build' task

#### - Add a Post-build step to the 'WebApp Build' task

    Goto Jenkins->'WebApp Build' task
    Under 'Post Steps'
    select 'Run only if build succeeds'  
    select 'Add post-build step'  
    select 'Invoke Top-level Maven Targets'  
    'Maven Version' 'maven' 
    Goal 'package'  
    'pom' 'CounterWebApp/pom.xml'  ## advanced button
    Click 'Apply'/'Save' button

#### - Run task and fix to ensure it works

    Run the task and ensure it works. Check console to see what is happening behind the scene.  Locate where the war file is .


### Task 4. Create Task to Deploy to Prod.

- Deploy package file to prod
- Test that it works

#### - Add a Post-build step to the 'WebApp Build' task

    Goto Jenkins->New Item->FreeStyleProject 'Deploy to Prod'
    Under 'Build' -> Execute shell
    `scp path_to_CounterWebApp.war prodserverip:/usr/local/apache2/tomcat8/apache-tomcat-8.5.13/webapps`
    `rm path_to_CounterWebApp.war `
    Click 'Apply'/'Save' button

#### - Run task and fix to ensure it works

    Run the task and ensure it works. Check console to see what is happening behind the scene. 



## Todo

- in your Github javaweb project, update CounterWebApp/src/main/webapp/WEB-INF/pages/index.jsp file, change 'Message' to 'Info' and 'Counter' to 'No:', then run entire redeploymemt pipeline to see your change in prod
- Our Pipeline does not have Test task.. can you add this?
- How can you ensure you have a back up of your source code before every deploy?
- Can you include email/text alerts to your builds?

## Summary

We have looked at how to install and configure Jenkins and Git on a server, create a job item and troubleshoot.
