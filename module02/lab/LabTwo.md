# Lab Two: Provisioners

## Creating our first development environment

1. So now that we've been over the basics of Vagrant let's go ahead and create an application that demonstrates how we might use it in real life. Let's put up a quick single-page website! 

2. Before we start let's plan out what we need to do...SO- in order to save time let's just use one of the boxes that we already have (specifically the **hashicorp/precise32** box- which is to say....ubuntu). So for a checklist:

* **Environment:** ubuntu 32 bit
* **Web server:** apache2
* **Port:** 80 on the host
* **Host:** localhost

3. So for the environment- check...all set (We can check to make sure that the virtual machine is there by doing a `vagrant box list`...go ahead and do that now and you should see the **hashicorp/precise32** box there). 

4. NOW- we need to make sure that our virtual machine adds the program that we need. Specifically- we'll need an **apache2** program installed with our virtual machine. NOW- you __might__ be thinking "okay- so I'll just create the VM and add apache2 with `apt-get install -y apache2` manually and then save the machine!"

![wrong](../../images/wrong.jpeg)

5. REMEMBER- we are looking to create a server that can be __re-created__ by other developers (the **repeatable** aspect of ideal development environments from module one). This means that we need all of the code in one central location (let's call it something like a....**Vagrantfile**(?)). SO- we want apache listed as a dependency. Where do we do this? 

6. Go into your **git bash** console and `cd ~/Documents/Github/accelebrate-vagrant-docker/module02/lab`. 

7. Now we need to **initialize** a vagrant repository here....any idea how to do that? (hint: it's `vagrant init hashicorp/precise32`). This will, of course, create a **Vagrantfile** in your local repository.

8. Using **Notepad**- go ahead and open up the Vagrantfile. NOW- let's talk about the other two aspects of our plan...
    * BECAUSE we want to be able to see what our awesome website will look like in our browser we want to forward the port from our virtual machine (obviously virtual machines don't have nice, visual web browsers like CHROME so it would be a pain to try to see what your website looks like)...so this means that we will need the virtual machine to "take over" the web port of the HOST machine! 

8. The next thing we need to do is to make sure that, when we create our website, we can do effective **port forwarding**. As we know- the default http port on servers is port **80** (or port **443** for secure socket layers).

9. Let's go back into our **Vagrantfile** and update the file (around line 26) to the following (you probably only have to uncomment it): 
`config.vm.network "forwarded_port", guest: 80, host: 8080`

10. As one more extra bonus here- it would be somewhat useless to have this awesome setup with port forwarding if we didn't have a webserver to SERVE that port, right? In other words- it would be nice to install a, let's say, APACHE server that will handle incoming/outgoing connections from that port, right? 

11. So let's do that! Go down to the bottom of the vagrantfile and look for the section that starts with `Enable provisioning with a shell script`. Under that you should see a shell script that will automatically download and install an apache2 server for you. 

```shell
    config.vm.provision "shell", inline: <<-SHELL
        yes | sudo cp C:/Users/Administrators/ /var/www/html/index.html
        apt-get update
        apt-get install -y apache2
    SHELL
```

12. Please paste the above code into the `config.vm.provision` section.

13. Now with that uncommented let's go ahead and run and re-provision our vagrant machine and see what we've got! `vagrant reload`.

14. Let's navigate to `http://localhost:8080` and see if that worked!

15. **Congratulations on creating your first environment!!**

![youdidit](../../images/youditit.jpeg)
