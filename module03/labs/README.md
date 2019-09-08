# Lab Three: Providers

## Switching to a new provider

1. So before we go on let's review one of the main aspects of Vagrant that we talked about from the beginning..specifically that we haven't build __vagrant machines__....we've beein building __virtualbox machines__ USING __Vagrant__.

2. But __what if__ we wanted to build vagrant machines using another provider...like **VMWARE** or **DOCKER**? 

![mindblown](../../images/mindblown.png)

3. So you'll remember from our earlier module that in the vagrant file there is a **config.vm.box** section that lists what **provider** is being used. Well...to tie together our previous class (on docker) with this one (on Vagrant) let's switch providers to use DOCKER as the provider! 

4. Go into 