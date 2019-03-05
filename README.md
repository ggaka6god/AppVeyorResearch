“I am [Arnau Gallofré](https://www.linkedin.com/in/arnau-gallofr%C3%A9-649785180/), student of the [Bachelor’s Degree in Video Games by UPC at CITM](https://www.citm.upc.edu/ing/estudis/graus-videojocs/). This content is generated for the second year’s subject Project 2, under supervision of lecturer [Ricard Pillosu](https://es.linkedin.com/in/ricardpillosu/).”

## About Appveyor

When you are developing a videogame in a team, you are constantly adding content to the project, and in order to test it quickly and saving a lot of time doing manually releases, on 12 November 2014 Microsoft created a powerful tool called AppVeyor, an online app that does the releases for you automatically. There are other apps that does the same, like Travis, but we will focus on AppVeyor.

So, if you want automatic builds for your project, follow the steps below:

## Sign-Up:

This part is quite easy if you already have an Github account. Remember to select the FREE plan account. Then, just use your Github username and password. [Sign-Up Here](https://ci.appveyor.com/signup).

Now that we have both accounts synchronized, we will need to create a new AppVeyor project selecting the repository we want to have. But if we do a commit, we will have errors and the build will fail. This is because the configuration of the project is wrong. But don't worry, we will fix it right now.



## Configuration:

The key of AppVeyor is the file that you create when you finish the configuration. This file is called appveyor.yml and is the file that AppVeyor will read every time you do a commit. You can fill this file manually (Bad idea), or export it from AppVeyor once you have all set up (Great idea). Once you have this file, you only have to put it in the main folder of your repository, eith the readme and the licenses.

Before entering into the settings, let's add our repo to AppVeyor

Once done, let's start with the General section. Here you can set up the build version and in which branch you want to make the build, even though there are another settings. **REMEMBER TO ALWAYS GO TO THE BOTTOM OF THE SECTION AND CLICK ON THE SAVE BUTTON. IF NOT, THE CHANGES WILL NOT APPLY**.

The next section is Environment, where you have to select the version of Visual Studio that you are using.

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
