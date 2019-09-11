# Lab Three: Provisioners

## Setting up a provisioner

![provisioner](../../images/provisioner.png)

1. Up to this point we've been heavily focused on using the `SHELL` commands to do very simple provisioning onto our Vagrant machines.....and shell works very well....at least until our machines start getting fairly complex. 

2. In the quiz leading up to this section we went over creating a __very, very basic__ Django setup...and you saw how many lines that took up, right? Well imagine if you had to install 100 packages. Or 1,000? As your production machine grows more and more complex the complexity of these machines can grow and, well...we don't want incredibly long, complex, difficule **Vagrantfiles**, right?

3. So just consider that from an organizational perspective it would be a lot easier to keep the Vagrantfile separate from the provisioning code. The **Vagrantfile** could deal with things like the amount of memory, the Operating system, and maybe one or two basic commands while, in a separate section of our Github repos we could keep **provisioning code** in the form of **Ansible Playbooks** (or **Chef Cookbooks** or **Puppet Manifests** or whatever...)

4. The main point here is that these machines, created by Vagrant with just a tiny bit of code, are always the same and always download the same dependencies so that we get a stable environment!

5. In this section of Lab 3 you'll notice that we have a handy-dandy **provision** folder which is an ansible system that I set up for this class. You can explore through the sub-sections if you'd like but the main thing to focus on is **provision/roles/vagrant_test/tasks/main.yml** as this is the folder that will be __doing__ everything for us. 

6. Without going **TOO** deeply into Ansible here (this is a **Vagrant** class after all)- what's happening in that **tasks/main.yml** file is three steps:

    * INSTALLING an apache2 server (see the variable there? `"{{ vagrant_test.package }}"` ? Well look in the `defaults/main.yml` and you'll see the variable that it points to- our old friend **apache2**!)
    * STARTING the service- pretty self explanatory here; start up the webserver and
    * COPYING a new **index.html** file into the root folder for the apache2 web server.

7. SO IN SUMMARY- it's basically the same thing we did __earlier__ BUT...now it's in ANSIBLE and has room to grow!

8. SO- how do we prepare our Vagrantfile for all of this? Well- let's start by updating a few things:

* We'll need a forwarded port (this is a web application after all!). Let's forward to port **80** on the guest machine to **8086** on the host machine.
* We'll need to make sure we're using the right box...let's go with **bento/centos-7.2** (which we already have downloaded so it should save time).

9. Now head down to the **config.vm.provision** section. We're going to want to update that as follows:

```ruby
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "provision/playbook.yml"
    ansible.verbose = true
    ansible.extra_vars = {
      hello_web: {
        package: "httpd",
        service: "httpd",
        docroot: "/var/www/html"
      }
    }
  end
```

10. SO- a couple of quick things to note here: 

* First- you'll notice that we are able to use "ansible_local" as the provisioner and that it's **built in** to Vagrant...which is a HUGE advantage because it saves having to keep these pieces together. 
* Second: You'll notice that I'm overwriting some variables here. You remember how two steps ago I showed you the ansible file that had "apache2" as the **package** variable? WELL...in CENTOS that's known as **httpd**...so we're going to overwrite it directly from the Vagrantfile here.

11. Close out your vagrantfile and let's go with a `vagrant up` and see what happens next! 

12. Ideally (and because we chose "verbose") you should, after boot time, start to see the playbook being played out (meaning that the necessary commands are there). 

13. **CONGRATULATIONS!** You just successfully provisioned a Virtual Machine with a third party provisioner.

14. **FINAL NOTE**: If you don't know **ansible**, **chef**, or **puppet** I can't recommend them enough as a great way to organize your provisioning code. There will come a time when you might need to provision hundreds of servers at once to handle a large volume of data and these tools are __essential__ to keeping your environments healthy. If this is your first exposure to Ansible I'd be happy to walk through the logic of the playbook with you! 

## Creating a network of multiple machines

1. It is entirely possible- in this modern, cloud based era- that your particular production environment is set up in such a way that you are dependent on multiple machines to form a **network** to work.

2. The most common version of this is a single machine running your database (**mysql** or **postgres** or **mongo**...whatever you are using) and another machine running your **application code** (node.js, python, etc). **Django**, for example, depends on a back end server from which to form the objects that are used as models.

3. The good news is that with Vagrant we can emulate a network environment simply by downloading a couple of machines and running them locally! So, in other words- we can hook up box one to box two and increase our computing power through the power of networking. 

4. What we're going to do in this section is create a connection between two disparate boxes (both Centos7 though!) through a private IP network.

6. Now let's `vagrant destroy` our wonderful __ansible created__ box and `rm Vagrantfile && rm -rf .vagrant`

7. Okay- so let's bring up a multi-machine system in vagrant. __Little known trick__...this is easily done with `vagrant init bento/centos-7.2` again in this directory and take a look at our new **Vagrantfile**

8. To keep our code separated out let's use another neat aspect of the language of the **Vagrantfile** (**Ruby** if you are curious)- we are going to separate each machine with a `config.vm.define` block. So let's overwrite the `config.vm.box = bento/centos-7.2` with the following block of code:

```ruby
  config.vm.define "webserver" do |subconfig|
    subconfig.vm.box = "bento/centos-7.2"
    subconfig.vm.hostname = "webserver"
    subconfig.vm.network :private_network, ip: "10.0.0.10"
  end
```

10. So basically, what we have above, is the minimum required entries to define a machine (in this case our webserver). The **hostname** needs to be unique and then the **private ip** needs to be in the **10.x.x.x, 192.168.x.x, 172.16.0.0 - 172.31.255.255** range. In this case, for simplicity, let's keep it in the 10 range and go with "10.0.0.10".

11. NOW, below this we're going to define a second machine in our Vagrantfile that will act as our database:

```ruby
  config.vm.define "mysql" do |subconfig|
    subconfig.vm.box = "bento/centos-7.2"
    subconfig.vm.hostname = "database"
    subconfig.vm.network :private_network, ip: "10.0.0.11"
  end
```

12. Notice that our ip in "10.0.0.11" this time instead of the "10.0.0.10" in the last ones. This gives the second host a unique IP address.

13. Let's close up our Vagrantfile and bring everything up! `vagrant up` now!

14. Did you get an error? No worries- that was expected. As we brought up TWO machines this time we'll need to specify which of those machines we want to ssh into. Let's go into our webserver with `vagrant ssh webserver`

15. Once in let's see if we have our network up! The best indicator here is if we can ping the other machine! So let's `ping 10.0.0.11`. IF you get back pings then congratulations! Your machines are communicating!

![woohoo](../../images/woohoo.jpg)

16. NOW...I've placed a MYSQL ansible playbook in this module03 lab for you. This Ansible playbook WILL install MYSQL5.7 on any machine you want to point to. I have done this with a nefarious purpose

![villian](../../images/villian.jpeg)

17. Your mission (if you choose to accept it) is to provision JUST the mysql server with the attached **./mysqlplaybook/playbook.yml**. HINT- use the code from earlier in this lab.

18. THEN...once you have that __but before you ssh into the server__ we'll also want to add in a way to push stuff for our **webserver** as well SO...

19. **As a second challenge** I want you to add in, using shell provisioners, but __in the same Vagrantfile__ add in the following packages:

```shell
    sudo yum install -y epel-release
    sudo yum install -y python-pip
    pip install mysql-connector-python
```

20. NOW- as a final challenge (NOT TO BE DONE NOW BECAUSE IT IS A RABBIT HOLE) we can figure out later how to network these two together (hint- it involves IPTables and the `my.cnf` folder). BUT FOR NOW...realize that with __only this single Vagrantfile__ we have successfully put two computers together in a **private network**...which is kind of amazing.

![smart](../../images/smart.jpeg)

21. Now- just to be more of a villian- MOVE your Vagrantfile OUT and `rm -rf .vagrant` to get rid of all evidence that this was ever a Vagrant directory

![drevil](../../images/drevil.jpeg)

## Docker and Vagrant

1. SO-- let's talk through **providers** (to bring everything full circle). Obviously, so far, we've been using **virtualbox** as a provider...which is great BUT...to look WAAAAY back to when this five day class began...we can __also__ use DOCKER in the same way.

![mindblown](../../images/mindblown.png)

2. So there are two ways we can use Docker here: 

* We can use it as a PROVISIONER
* We can use it as a PROVIDER

#### Docker as PROVISIONER

1. So...using it as a **provisioner** basically means that docker will be installed ON the virtual machine (so...a virtual environment INSIDE your virtual...environment!)

![inceptionagain](../../images/inceptionagain.jpg)

2. Where is this useful? Well- maybe you want to see how your docker container will perform on your production host if you are using docker containers to manage your application. This is an __excellent__ use of a vagrant virtual machine. Let's make this happen now (though move on as soon as we do this because it might take a while here). Run your `vagrant init bento/centos-7.2`.

3. Open up your Vagrantfile here and just add this line in the **config.vm.provisioner** section:

```ruby
config.vm.provision "docker",
    images: ["django"]
```

4. So this is a neat little feature of Vagrant....it recognizes **docker** as a command line argument and will automatically download a docker image with the attached code. Keep in mind that the docker image is __inside your vagrant machine__ so it's not necessarily accessible from host...BUT...as a matter of convenience...as soon as you `ssh` INTO the host you'll see the image already there.

5. This can also be useful if you have a **Dockerfile** handy... if you want to (and you don't need to here...building a machine that provisions with Docker will take long enough) you can do the following thing to provision your vagrant machine with your docker container built and raring to go:

* MOUNT your dockerfile into your machine HOME directory (remember how we do that?)
* Run the `docker build` command

All of this will automatically happen if you add this code to your vagrant machine:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provider "docker" do |d|
    d.build_dir = "/home/vagrant"
  end
end
```

6. Anyway- if you've built your docker image you should be good to go now. Let's `vagrant destroy` the current box and `rm Vagrantfile && rm -rf .vagrant`.

#### Docker as PROVIDER

**Note that we are not going to be running this as a live LAB**

1. Another way we can use Docker with Vagrant is that DOCKER can actually PROVIDE virtual machines for vagrant to run!

2. Now- ultimately there are issues that come up with installing Docker on Windows Servers...so we have to skip that here as docker has system requirements that we don't meet using a virtual machine.... but it's essential to understand that you can use the following to have DOCKER act as a provider:

```ruby
Vagrant.configure("2") do |config|
  config.vm.provider "docker" do |d|
    d.build_dir = "."
  end
end
```

3. What the above code will do is build a docker image as a vagrant machine based on a dockerfile in the same directory



