## Pre-requisite 
 - We need to have Jenkins and sonarqube server up and running (by default Jenkins runs on 8080 and sonarqube at 9000), to install on ubuntu refer [link](https://github.com/DeekshithSN/cheatsheet/blob/master/installtion_guide_ubuntu.md)


## Initial setup 
In Jenkins plugins like docker, sonarqube and Sonar quality gate plugin (manage Jenkins --> manage plugins --> in available tab search for respective plugins)
After sonar plugin installation in configure system enter sonar information as shown in below picture 

Name: any meaningful name, but this will be reffered in pipeline while executing sonar steps 
Server URL: sonarqube url
Sever authenctication token: this tocken has to be created in sonarqube
To create that tocken in sonarqube navigate to administration  Security  Users ( after which you will see list of users)
 
Jenkins Sonarqube integration for static code analysis 

Pre-requisite 
1.	We need to have Jenkins and sonarqube server up and running (by default Jenkins runs on 8080 and sonarqube at 9000), to install on ubuntu refer link
Initial setup 
In Jenkins plugins like docker, sonarqube and Sonar quality gate plugin (manage Jenkins  manage plugins  in available tab search for respective plugins)
After sonar plugin installation in configure system enter sonar information as shown in below picture 
 
Name: any meaningful name, but this will be reffered in pipeline while executing sonar steps 
Server URL: sonarqube url
Sever authenctication token: this tocken has to be created in sonarqube
To create that tocken in sonarqube navigate to administration  Security  Users ( after which you will see list of users)
 




Click on tocken button, you will prompted to create tocken as shown in below pictures  
 
Give meaningful name and click on generate 
 

Copy the token to create secret text in jenkins 
 



In Jenkins navigate manage jenkins  manage credentials  add credentials  give value in secret ( will show up in sonar authentication tocken dropdown 
 
Also need to create webhook to have communication between jenkins and sonarqube
To create webhook navigate to administration  configuration  webhooks  then provide name and url of your jenkins sufixed with /sonarqube-webhook/
 

Then in sonarqube configure quality gates, below is default if want we can change it 
 


Then create jenkinsjob with code DeekshithSN/sample-web-application at sonar-jenkins (github.com)
Jenkinsfile would look like this, as you can see we are verifying our code against sonarqube quality gate, it code satisfies with quality gate then it will procced further else it will stop build in that stage 
 

If you are quality gate satifies then you will see passed in jenkins page 
 
