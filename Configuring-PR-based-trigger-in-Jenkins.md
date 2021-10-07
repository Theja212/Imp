## Pre-requisite 
- Jenkins server and github repository 


## setup 

we need to install plugin which are mentioned in below table 

Plugin	| Version
---------|---------
GitHub Pull Request Builder | 1.42.1
Git | 4.2.2
GitHub Plugin |	1.30.0
GitHub API | 1.114.1

## GitHub Pull Request Builder
This is the plugin that handles everything related to your pull request.
For me, all traditional approaches didn't work and this was the only plugin that actually did what I wanted it to do.
Keep in mind: There might be other ways to achieve this!

This plugin will expose a webhook which GitHub can later use to send meta data to.
The webhook is exposed at: ```<yourJenkins/>/ghprbhook/```
A request arriving at the hook is then used to identify a Jenkins build which is actually run then.
The plugin will (when configured):

- add a comment to your PR and ask for a review
- add a merge check which you can add to the branch protection requirements for a review
- start the build and send the result back to GitHub using its API

## Adding Credentials For Authentication With GitHub
You will need credentials stored in Jenkins for two things:

Pulling your source code from GitHub
Using the GitHub API to make comments and push the result of the merge check
First you have to add some credentials to your Jenkins so that it can later authenticate requests to GitHub.
In the main menu of Jenkins, click on "Credentials":

![github1](https://user-images.githubusercontent.com/29688323/136421254-5b038310-3844-497e-a505-c69931fc7479.jpg)

Choose a scope. The global scope is okay. If you have another one you want to use for it, feel free to do so!
When you have chosen, click on the scope.


![github2](https://user-images.githubusercontent.com/29688323/136421535-a6f2acac-2493-4016-b201-8f9857657c0f.jpg)

Click on "Add Credentials", now.

![github3](https://user-images.githubusercontent.com/29688323/136421530-0f9337fc-44d1-4766-bb62-d6389f3feb7f.jpg)

On the following page, choose the kind "Username with password" and fill out all the necessary information.
After this, click on the "OK" button and you are finished here.

![github4](https://user-images.githubusercontent.com/29688323/136421538-deb1bf21-2a64-4878-a8f8-e62a884302d3.jpg)

You could also work with an API token here, but for the sake of simplicity, username and password is just more straight forward.


## Setting The GitHub Project URL

Before doing anything more, enter your GitHub project URL.
Many posts or articles do not mention this enough, but the url is part of the routing mechanism, that decides what project to actually build when your webhook receives a message.
If this field is left blank, no build will ever run!

![github5](https://user-images.githubusercontent.com/29688323/136421945-438d93b3-b9ef-429c-adc3-1e35e3524690.jpg)

## Adding The Repository
In order for your Jenkins to actually build your project, you have to add the git coordinates.
Go to your project and just copy the HTTPS clone link.
After that, choose the credentials you created earlier from the dropdown.
In your case, that red error message should, of course, not pop up.
If it does, there is something wrong with your credentials (check your password again!) or your Jenkins can't reach GitHub.

![github6](https://user-images.githubusercontent.com/29688323/136422302-7ad87afc-836a-40c7-b05c-f086e57d3a78.jpg)

The goal is to build individual Pull Requests before they are merged into your main branch. As they originate from another branch as your main one, Jenkins somehow needs to get told which specific branch to build.
By adding a refspec and a branch specifier, including variables that are set by the plugin, the job will always build what is specified by the webhook trigger, that comes from GitHub itself.