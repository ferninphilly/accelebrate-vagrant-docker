# Review from Module Two

1. Go into **git bash** and go into the exercise directory that we created at the end of Module one

2. Open up that vagrant file (with the CENTOS machine) create a provisioner __runs an update__ and then downloads the following programs:

* epel-release
* python-devel
* python-setuptools
* python-pip
* pip install --upgrade pip
* pip install virtualenv
* pip install django

3. Forward the port from the host machine on to the vagrant machine through port **3000**

4. Using the "make directory" command in git bash, create a directory called "web_app" on the host machine under the exercise directory.

5. In the Vagrantfile go ahead and MOUNT that directory onto your virtual machine through the vagrantfile.

6. Go INTO the virtual machine through `ssh`

7. From WITHIN the vagrant machine run `touch djangofile.py`

8. **Exit** the virtual machine and from the host see if the **djangofile.py** exists on the host machine (demonstrating two way sync)

