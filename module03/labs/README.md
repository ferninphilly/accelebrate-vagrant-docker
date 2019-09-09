# Lab Three: Provisioners

## Setting up a provisioner

![provisioner](../../images/provisioner.png)

1. Up to this point we've been heavily focused on using the `SHELL` commands to do very simple provisioning onto our Vagrant machines.....and shell works very well....at least until our machines start getting fairly complex. 

2. In the quiz leading up to this section we went over creating a __very, very basic__ Django setup...and you saw how many lines that took up, right? Well imagine if you had to install 100 packages. Or 1,000? As your production machine grows more and more complex the complexity of these machines can grow and, well...we don't want incredibly long, complex, difficule **Vagrantfiles**, right?

3. So just consider that from an organizational perspective it would be a lot easier to keep the Vagrantfile separate from the provisioning code. The **Vagrantfile** could deal with things like the amount of memory, the Operating system, and maybe one or two basic commands while, in a separate section of our Github repos we could keep **provisioning code** in the form of **Ansible Cookbooks**


2. But __what if__ we wanted to build vagrant machines using another provider...like **VMWARE** or **DOCKER**? 

![mindblown](../../images/mindblown.png)

3. So you'll remember from our earlier module that in the vagrant file there is a **config.vm.provider** section that lists whether we want the visual gui from virtualbox or just to allow ssh access to the machine. To remind you- we initially went with `vb.gui = true` 

4. So we're going to switch providers in that section. Let's go with **Docker** in order to bring everything full circle and show how **Vagrant**

5. **Generally**- when choosing Virtual Machine providers- you will want to go with a vm provider that lines up as closely with your host OS as possible. There are a lot of issues that can be avoided if we follow this general rule. In this case **Hyper-V** is the microsoft virtualization provider. BUT (as with most things with windows machines)

6. So let's start by adding in our vagrant box manually BUT..unlike what we've been doing to this point let's explicitly call out the provider. SO...let's run this command: `vagrant box add hashicorp/precise64 --provider hyperv`.

7. Now...this could take a while so let's go through some of the other aspects of hyperv boxes while we wait for everything to download...

8. Another thing to understand about selecting the "provider" is that, because you can have multiple providers for the same box, you need to explicitly call out the provider when you are `vagrant up` -ping the box.

9. SO- before we are able to effectively do this we'll need to `vagrant destroy` the current box in our directory...so run a `vagrant destroy` now.

10. Okay- so if everything is ready to go we should be ready to run everything **except**...

![watchout](../../images/watchout.png)

11. Here's the thing....VirtualBox and Hypervisor __can not__ run simultaneously. Without going into the long, painful, technical aspects of setting up virtualization on windows machines- let's just take it as given that we'll need to **turn on** the hypervisor on windows in order for this to work. 

12. BUT...let's ALSO take it as given that when we **turn ON** hypervisor on the windows machine we'll also be **turning OFF** the ability for virtualbox to do it's thing. Sorry- but them's the breaks. 

13. Anyways- to turn on the hypervisor (and this could require a machine restart) open up **powershell** on the windows machine and put the following commands into your powershell window: 

`DISM /Online /Enable-Feature /All /FeatureName:Microsoft-Hyper-V`
`Install-WindowsFeature -Name Hyper-V, RSAT-Hyper-V-Tools`

![dismupd](../../images/dism_upd.png)

14. Go ahead and run the restart. Now go back into git bash and 