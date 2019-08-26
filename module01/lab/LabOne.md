# Lab One: Setting up the provisioners and providers

# Setting up your vagrant environment

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