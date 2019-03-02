“I am [Arnau Gallofré](https://www.linkedin.com/in/arnau-gallofr%C3%A9-649785180/), student of the [Bachelor’s Degree in Video Games by UPC at CITM](https://www.citm.upc.edu/ing/estudis/graus-videojocs/).
 This content is generated for the second year’s subject Project 2, under supervision of lecturer [Ricard Pillosu] (https://es.linkedin.com/in/ricardpillosu/).”

Introducing AppVeyor
In the development of video games, every time a feature is added to the game in progress, it has to be tested to look for bugs or design issues, to do it correctly, the developers should upload a release with the new feature added to GitHub, and then the QA of the team will check for these errors. So in order to save time and not to make every time a built for the release manually, it’s recommended the usage of the free application AppVeyor. We will focus on how to use AppVeyor, but there are other similar applications that share the same results, like Jenkins or Travis CI.

AppVeyor, every time a commit is done to the code, it automatically uploads the build with all the needed artifacts to the Release page of GitHub giving you feedback of how the build has been done.

So, in this article we will talk about how to configure AppVeyor to do automatically builds to GitHub. Following this guide step by step we will understand how AppVeyor works and how to configure it to obtain our desired results.

I recommend this guide of How AppVeyor works to understand the internal processes that it does. Even it is not necessary to understand the process we will make in this article.

Let's start with the Tutorial:
Starting with AppVeyor
● To begin with, we will need to create an account on AppVeyor using our GitHub account.

● Then, we will need to authorize it. If the GitHub repository to apply it is from an organization, it will require the authorization of the organization's owner. So it’s recommended to be done by the owner himself.

hi

● Once we have synchronized both applications, we can go on with AppVeyor creating a new project and selecting the GitHub repository which we want to have automated builds.

● Now we have our project in AppVeyor, by default every time we make a commit, it will try to make a built, but it probably fails due to the app configuration is not the correct. So the next step is how to configure it.

Configuring AppVeyor
First of all you need to know that there are two ways to configure an AppVeyor project. The first one it is found in the project itself, in the Settings section; there, there is an interface to help you configure the whole project.

hi

The second one is creating in your Github repository a YAML file named appveyor.yml where is found the same configuration but in YAML format.

hi

I highly recommend first of all configure your project from AppVeyor project, in the Settings, and then export this changes into an appveyor.yml and added into your GitHub repository. So you can change any setting from the repository.

It’s needed to remark that AppVeyor will give preference to the YAML file before the project settings. So be careful on that.

Here you can check the full documentation of all the possible settings that the project has.

But in this article we will focus on the basic settings to complete our objective, that is automated builds. The project settings is divided in different sections, the main one is General. There, the most relevant option is that you can configure the Build version format, that will increase every time a built is done (regardless of if it fails). Another useful setting is that you can select from which branch you want to make the built every time a commit is done, in Default branch and Branches to build.

hi

The next important setting is found in Environment where you have to select which Visual Studio version are you using.

hi

Another relevant option is that one that uploads the build done to our GitHub releases page. It is found in Deployment and is need to change the deployment provider to GitHub Releases. It is recommended to add a Release description and mark the Draft Release to avoid having all the releases you made there. But before all of that is needed an authentication from GitHub to let AppVeyor modify our repository. It is done through a GitHub authentication token.

hi

We will make a parenthesis to show how to get it:

How to get your GitHub authentication token
First of all, it’s important to say that an authentication token is like a password, so manage them like that. The difference is that it is used for scripts or commands, and in addition you can revoke them and generate lots of them. So, to generate one of them you need to go Here or manually going to your GitHub, and go to Settings (the general settings, not the repository ones). There is a section Developer Settings with a subsection Personal Access Tokens.

hi

There you need to Generate a new token and just select the scope public_repo and then Generate it.

hi

Once done the token has to be copied to Here to encrypt it, the result is an encrypted token that has to be copied to the GitHub authentication token in the Deployment setting that we were talking before.

Back to AppVeyor
At this point AppVeyor is capable to access to the Release GitHub page.

So our objective is to make AppVeyor do automated builds from our GitHub repository, but we need to remind which items a build should have:

A README.md file
A folder with all the Assets of the game and the libraries .dll
The executable of the game .exe
It is recommended putting together in a folder the ReadMe, the assets and the libraries to make the process easily. In all the explanation we will refer to this folder as \Game.

So we need to upload this Game folder together with the executable of the game, that will be given by AppVeyor once it has made the Release. To do it we need to go again to the project Settings. Firstly we need to go to Build section and fill the Configuration option with Debug and Release. We put both, to check that there’s no problem compiling the code in Debug nor Release mode. Then, as we said before, we need to get the executable given after the AppVeyor does the Release to our code. So in Before packaging script we need to insert a script in PS (PowerShell) language which will copy this executable to the Game folder, to have it all together.

The script is the following:

Copy-Item C:\projects\(your_project_name)\$env:CONFIGURATION\(your_solution_name).exe C:\projects\(your_project_name)\Game\.
"Where the directory to copy is the solution of your Debug/Release, and the directory to paste it is your Game carpet"

"($env:CONFIGURATION): refers to Debug or Release folder"

Below we have an example

hi

So now, after it is done the Release to our code, the folder Game will contain all that a correct built needs. So the last step is to upload this folder to our GitHub Release page. It is configured in the section Artifacts where we need to put the path to the folder Game, with a name for the release and selected the Web Deploy Package type.

hi

If the steps are followed correctly the build should be uploaded to Release page as a draft every time a commit is done in the project. I recommend, as we said before, to export all the configuration to YAML format and upload to the repository to allow the modification directly from GitHub.

hi

Another useful feature that AppVeyor provides is Notification, every time a built is done it will notificate it through the channels that you prefer, the most common are Email, Slack.. There, you can select when it has to notificates you. Whether the built has been upload successfully or it failed, or both.

hi

Links to more documentation
Official AppVeyor Tutorial

Documentation of the configuration of AppVeyor

How AppVeyor works
