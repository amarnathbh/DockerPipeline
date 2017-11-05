# DockerPipeline
This project is to build DevOps Continuos Integration pipeline using Jenkins and Docker

Main Requirements:
1. Jenkins
2. Docker
3. Git
4. Java
3. Two virtual machines: master and slave. 

This project covers following things:

* The ability to trigger a build in response to a git commit via a [git hook](http://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks).
* The ability to determine failure or success of a build job, and as a result trigger an external event (run post-build task, send email, etc).
* The ability to have multiple jobs corresponding to multiple branches in a repository. Specifically, a commit to a branch, `release`, will trigger a `release build job`. A commit to a branch, `dev`, will trigger a `dev build job`.
* The ability to track and display a history of past builds (a simple list works) via http.

Code used for this project: 
https://github.com/amarnathbh/hello-jenkins 

#### Build
  1. Use this link to install Jenkins on Centos
[Installing Jenkins on Centos](https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+on+Red+Hat+distributions)
  2. Install this dependencies on slave node 
[Installing Dependencies](https://github.com/amarnathbh/hello-jenkins/dependencies.sh)

#### Configuring Master node
  1. SSH in to master node and change the sudoers file using visudo so that you avoid getting this error
      "sudo: no tty present and no askpass program "  default !requiretty " 

  2. Add user to sudoer group

#### Configureing Jenkins

 1. Login in to Jenkins console 

 2. Install the plugin
      1. Go to Manage Jenkins >> Manage plugins >> install (search and install various required plugins like github, email extension for notification, build pipeline etc)
      
      ![Screenshot](https://github.com/amarnathbh/DockerPipeline/blob/master/screenshots/plugin.png)
       
 3. Congfigure the slave node
 
      1. Give the name for slave node, executors and give Remote root directory.
      2. configure using below screenshot
      
      ![Screenshot](https://github.com/amarnathbh/DockerPipeline/blob/master/screenshots/configslave.png)
      
#### Creating a new job in jenkins

 1. Create a new job. Give a name to job. Select Freestyle Project.

      ![Screenshot](https://github.com/amarnathbh/DockerPipeline/tree/master/screenshots)

 2. Select github project and give Github url

 3. click Restrict where this project can be run and give label expression which is mapped to slavenode

 4. Select Source Code Management as Git and give github url and select the branch you want to build.

#### Automatic building 

 1. Go to your Job >> Configure >> Build Triggers. 

 2. Select Build trigger as GitHub hook trigger for GITScm polling and poll scm

 3. For Build select Execute shell and give command as below
...

      # build docker image using github repository
        docker build --pull=true -t github.com/amarnathbh/hello-jenkins:$GIT_COMMIT .
       # test docker image using test file 
        docker run -i --rm github.com/amarnathbh/hello-jenkins:$GIT_COMMIT ./script/test
...

#### Notification for build success or failure for a job

 1. Go to Manage Jenkins >> Configure System.

 2. Setup Extended E-mail Notification. The below screen shows setup for Extended Configuration. Both the setups are same.

    ![Screenshot](https://github.com/amarnathbh/DockerPipeline/blob/master/screenshots/Emailsetting.png)

 3. Go to your Job >> Configure >> Post Build Action >> Select Email notification and editable Email notification

      ![Screenshot](https://github.com/amarnathbh/DockerPipeline/blob/master/screenshots/EmailNotification.png)
      
 4. Now, when you build you will get the email notification for the corresponding build.

      ![Screenshot](https://github.com/amarnathbh/DockerPipeline/tree/master/screenshots)
      
#### Triggering other job from this job   

 1. Go to your Job >> Configure >> Post Build Action >> Trigger parameterized build on other project

      ![Screenshot](https://github.com/amarnathbh/DockerPipeline/blob/master/screenshots/Triggering%20other%20jobs.png)
      
      ![Screenshot]()
      
#### To view History of past build

 1. View entire history using browser.

 2. Type "http://amarnathbh6b.mylabserver.com/job/Myfirstdockerbuild/api/json?pretty=true"

      ![Screenshot](https://github.com/amarnathbh/DockerPipeline/blob/master/screenshots/ViewHistory.png)




Sources:
1. CSCDevOps Course (https://github.com/CSC-DevOps/Course/blob/master/Project/M1.md)
2. Docker use case (https://www.docker.com/sites/default/files/UseCase/RA_CI%20with%20Docker_08.25.2015.pdf)
3. Jenkins.com


      
