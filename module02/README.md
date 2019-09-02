# Review from Module One

1. Create a new directory called **/Users/Administrator/myvagrantdir**

2. Change directories into that new directory using the powershell `cd` command and initialize a new Vagrant directory there.

From the main directory here find and download the most recent CENTOS/7 box from Vagrant (starts with centos...) using the `vagrant box` command to download the new vagrant image.

3. Go into your Vagrantfile and edit it to use the box that you just downloaded.

4. Edit the vagrant file to forward the port of the box onto the host system (NOT allowing it to be accessible to the internet)

5. Use the command to boot the vagrant machine with your changes in place

6. Go into the virtual machine and paste this command `echo "OH HAI MARK!"`

7. Exit the virtual machine back to your host machine.

8. Edit the Vagrantfile so that on boot it provisions with an update and upgrade (remember to use the proper package manager with CENTOS to do this). 

9. Edit the Vagrantfile to add in an `nginx` we server on boot.

10. Bring the virtual machine back up in such a way that it runs the provisioner again.

11. Suspend the machine. 

12. Halt the machine and restart it.

13. Destroy the machine.


