# Prerequisite Setup

## Set up the Virtual Lab

1. Using the link provided by accelebrate- start up and log into your windows image. 

![virtualwindows](./images/virtualwindows.png)

2. Congratulations! You've got a virtual machine! Let's start setting up virtual machines on your virtual machines!!

![inception](./images/inception.jpeg)

## Download and set up GIT 

1. Let's get Github for windows [here](https://desktop.github.com)- or...because we don't have it yet- it's **https://desktop.github.com**

2. Once it's downloaded go ahead and sign into your github account 

![gitsignin](./images/gitsignin.png)

3. Now that you are signed into github we want to **clone a repository from the internet**. That should bring up another window. Do a search for **ferninphilly/accelebrate-vagrant-docker**. You can leave all of the options as the default. Click "OKAY" to clone it.

![cloningrepo](./images/cloningrepo.png)

4. Now open up your file directories and navigate to the directory **C:\Users\Administrator\Documents\Github\accelebrate-vagrant-docker**. If you see the **accelebrate-vagrant-docker** directory then congrats!! You've got GITHUB!!

![congrats](./images/congrats.jpeg)

## Download and set up GIT BASH

1. Now that we have this code base inside our directories we can (hopefully) start using links! Let's get git bash by going [here](https://gitforwindows.org) (or we can use https://gitforwindows.org).

2. Download that one and double click it. When it opens up the installation windows there **are** two changes we need to make:

    * "Use Git from the Windows Command Prompt" with this option you will be able to use Git from both Git Bash and the Windows Command Prompt.
    * "Checkout as-is, commit Unix-style line endings"

![gitbash](./images/gitbash.png)

3. Everything else can be default (if you mess up- don't worry...just re-install). Once you're done with that go ahead and click on "Launch Git bash" at the end (if you haven't launched then you can search from Windows and do it from there). 

4. Launch a git bash window. Congrats! Third step done!

## Download and set up VirtualBox

1. Now onto VirtualBox- which is one of the **providers** (we'll be getting to that later) we'll be using for our Vagrant machine. Head [here](https://www.virtualbox.org/wiki/Downloads) (that's https://www.virtualbox.org/wiki/Downloads) and download the windows virtualbox executable.

2. Double click on the exe and run through all of the defaults (it's fine) including installing the network drive.

3. Do a quick search and launch virtualbox (just to make sure it worked!). Screen should look like this:

![vbox](./images/vbox.png)

## Download and setup Vagrant

1. Head [here](https://www.vagrantup.com/downloads.html)- which is https://www.vagrantup.com/downloads) and choose the appropriate 64 but system version. 

2. Download the MSI and install. All defaults again is fine (you have to accept the agreement)

3. Grab a cup of coffee and settle in...this could be a while...but once it's done go ahead and restart (you should be presented with the option).

4. NOW- once your VM has restarted go ahead and open up a **git bash** terminal (you can do a search for them and open up). Once that's open type in `vagrant -v`. If you got a response...congratulations!! We're good to go!!

![goodtogo](./images/goodtogo.png)