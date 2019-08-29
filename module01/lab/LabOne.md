# Lab One: Setting up the provisioners and providers

## Setting up your vagrant environment

1. So as we discussed in the presentation- the important thing to remember about vagrant is that it is **not a virtual machine**. It is an abstraction layer that allows us to **run** virtual machines.

2. This class focuses primarily on using **docker** as the provider...but it's also useful to look at other providers such as **virtualbox** and **vmware** as possibilities. 

3. Let's start by going over our main vagrant commands. As step one- start from the root directory of this github project and change directories into **./module01/lab**. So run the command `cd ./module01/lab`. 

4. Once in here we're going to run our first key vagrant command: `vagrant init`. This should result in a file appearing that says **Vagrantfile** in that directory. 

5. Let's take a quick look at the **Vagrantfile**: Essentially when you open it you'll see a lot of the explanation as to what should happen here commented out...but it should give you an idea of what's going on with Vagrant. 

6. So the first uncommented section is `Vagrant.configure("2") do |config|`- which starts a code block which is basically saying "DO A THING" (the "2" has to do with versioning).

7. `config.vm.box = "base"` is the next uncommented section. Let's take a quick look at this! Essentially what's happening here is that we are naming the type of virtual machine we want to create. With a default `vagrant init` we will get a box called `base` with a very, well..."BASE" operating system installed.

8.  Obviously this is not as useful as it __could__ be... this is just a very basic image. NORMALLY we would want a virtual machine that actually has some sort of OS- maybe **ubuntu** or **CentOS**? So before we go on let's change the `vm.box` to have an actualy, __operating system__ on it! 

9. First let's delete the current Vagrant file (on linux simply do `rm Vagrantfile` from the command line). Now instead let's get a new box- `vagrant init ubuntu/trusty64`. This should make a new `Vagrantfile` appear. 

10. NOTE that if you don't want to bother deleting the **Vagrantfile** you can do `vagrant init -f ubuntu/trusty64` which forces an overwrite of the current vagrantfile in the directory.

11. Now let's go into our **Vagrantfile** and you should now have a `config.vm.box = "ubuntu/trusty64"` which is a way of creating a ubuntu virtual machine. 

12. Our next command is to actually bring it to life! From the same directory run this command: `vagrant up`. You should see the image (thing ISO image) being downloaded. Once downloaded you shoul see a message like this one:

[!vagrantup](../images/vagrantup.png)

13. Note the default port forwarding! Port 22, in the vast, vast majority of networks, is reserved for ssh access to a machine (we can edit that later in the Vagrantfile). This means that access to our virtualmachine will be through port 22- which means ssh access. Vagrant has also done us the favor of copying the ssh key into the `authorized_keys` doc in the `~/.ssh` folder so now we only have one command left to access our VM:
`vagrant ssh`

14. Are you in your VM? Congrats! We've accomplished the first check!!

## Creating the environment

1. First off, while we're in our new Ubuntu machine, we should run a couple of basic checks. Run our basic ubuntu update command (`sudo apt-get update && sudo apt-get upgrade`). We can also add in any packages we want with `sudo apt install...`)

2. Now let's get out of our VM...which is, fortunately, very simple to do. On the command line simply type `exit`

3. Okay- now let's mess around with some other aspects of Vagrant to show how it can be used as a simple environment to develop a small website. If we were going to do this what are some things that we would want to include? Well...if I was in the middle of development I would probably want to be able to mount folders pretty simply. So how can we do this in a straightforward method? 

4. Go into your Vagrantfile and find `config.vm.synced_folder`. This is the folder that will be mounted onto your ubuntu machine when you start it up. Now...I have code for a very basic website here in the folder known as "app". In this example we're going to deploy the app on our localhost. To start this obviously we need access to the "app" folder from within the machine SO...let's update (and obviously UNcomment) the "synced_folder" as follows: `config.vm.synced_folder "./app/index.html", "/var/www/html/index.html"`.

5. Let's do another `vagrant up && vagrant ssh` and see if everything worked! Once in there do a `pwd` to see what directory you are in (should be **/home/vagrant**) and then do an `ls` to see what files are in there with you. Did it work?

6. Wait...it didn't? If it worked and it was mounted you SHOULD see a new folder in your home directory...but that seems to be missing? Why is that?

7. So basically our machine never stopped RUNNING. If we want the machine to re-read the vagrantfile we need to **reload** it. Run this command: `vagrant reload` and see what happens. You should see a restart on the machine and then a re-provisioning of the virtual machine. Now that you have **reloaded** the machine run a `vagrant ssh` to go into the machine and then an `ls` to see what's in there. Did your website make it in there this time?

8. The next thing we need to do is to make sure that, when we create our website, we can do effective **port forwarding**. As we know- the default http port on servers is port **80** (or port **443** for secure socket layers).

9. Let's go back into our **Vagrantfile** and update the file (around line 26) to the following (you probably only have to uncomment it): 
`config.vm.network "forwarded_port", guest: 80, host: 8080`

10. As one more extra bonus here- it would be somewhat useless to have this awesome setup with port forwarding if we didn't have a webserver to SERVE that port, right? In other words- it would be nice to install a, let's say, APACHE server that will handle incoming/outgoing connections from that port, right? 

11. So let's do that! Go down to the bottom of the vagrantfile and look for the section that starts with `Enable provisioning with a shell script`. Under that you should see a shell script that will automatically download and install an apache2 server for you. 

```shell
    config.vm.provision "shell", inline: <<-SHELL
        yes | sudo cp /home/vagrant/website_data/index.html /var/www/html/index.html
        apt-get update
        apt-get install -y apache2
    SHELL
```

12. Please paste the above code into the `config.vm.provision` section.

13. Now with that uncommented let's go ahead and run and re-provision our vagrant machine and see what we've got! `vagrant reload`.

14. Let's navigate to `http://localhost:8080` and see if that worked!

15. **Congratulations on creating your first environment!!**

[!youdidit](../../images/youdidit.jpeg)

## Vagrant Boxes

1. So- this is a nice beginning and hopefully now you can see where Vagrant is __incredibly__ useful for bringing up virtual machines!  One of the bigges differences between Vagrant and Docker is obviously that Vagrant is intended to be a more permanent solution. Unless you ran `vagrant halt` or `vagrant destroy` the machine is probably still running in the background.

2. To review- these are the commands that we have used thus far and probably the most common ones you'll be using:

`vagrant init`: Creates the Vagrantfile in a directory
`vagrant up`: Basically takes whatever is in the Vagrantfile and builds a machine from it.
`vagrant ssh`: Go into our vm via ssh
`vagrant reload`: It's the equivalent of running `vagrant halt && vagrant up` and will reload and re-provision the vagrant machine. Basically- this is what you'll need to run if you make changes to the Vagrantfile.
`vagrant reload --provision`: Forces a complete re-provisioning of the machine
`vagrant stop`: Stop your vagrant machine
`vagrant suspend`: Suspends your virtual machine and remembers the current state
`vagrant resume`: Resumes your stopped vagrant machine
`vagrant destroy`: Destroys your vagrant machine (so it will need to be re-provisioned)

3. So let's talk about how to get different kinds of machines! In the example above we set up a straight UBUNTU machine and apache server...but __what if__ we wanted a different **type** of machine? Well- fortunately there is a repository for that!

4. Click [here](https://app.vagrantup.com/boxes/search) for a quick look at the main Vagrant Repo of different offerings of images from which to build virtual machines. Using these is as simple as adding a line to your vagrant file OR (as we shall see) pulling them directly from the repository with `vagrant box add`

[!prettycool](../../images/prettycool.jpeg)

5. So these are __images__ of machines that Vagrant can run! Some of them come with a lot more than just straight Operating Systems. As you can see you can get all sorts of different machines here...a lot of them pre-loaded with some pretty cool stuff (like Docker images in the Dockerhub repo, right?). We went over images in the powerpoint of course. 

6. So let's do a quick search for a **Django** box by typing **Django** into the search bar and see what comes up...

[!djangoboxes](../../images/djangoboxes.png)

7. So- from speaking during our scoping call it would appear that we might need to look at a django/rhel7 (CentOS) combo to work from. After doing a search [here](https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=downloads&provider=&q=Django%2C+centOS) is where I landed. Looks like a Django 1.6 with CentOS...which should work well for our needs... so let's take a closer look:

[!centosdjango](../../images/centosdjango.png)

8. Go ahead and click on the `sapphirus/django` vagrant image and take a closer look. You'll immediately notice how easy it is to add to your vagrant boxes by simply adding it to the configuration section of your vagrant file....BUT....

9. Let's say that we wanted to **download** this one but didn't want to **keep** it. You see-- what happens with a Vagrantfile is a lot like what happens with docker- to whit: When we say "I would like a docker image of Django" or "I would like a vagrant image of Django" the first thing that happens is that docker and vagrant check **locally** to see if they can find the image locally. __If they cannot__ find the image locally then they will hit the vagrant repo here and download it.
