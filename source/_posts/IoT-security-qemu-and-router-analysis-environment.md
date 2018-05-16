---
title: 'IoT security: qemu and router analysis environment'
date: 2018-03-03 23:16:09
tags:
- IoT security
- 2018
---

I am learning IoT security these days and just set up an environment for router file system analysis. So I will write a tutorial for you to save some time.

I am using Ubuntu 16.04 here.

First, we will install the qemu.

{% codeblock lang:bash %}
sudo apt-get install qemu 
sudo apt-get install qemu-user-static
sudo apt-get install qemu-system
{% endcodeblock %}

Then we have to install some network tools and configure the interface.

{% codeblock lang:bash %}
sudo apt-get install bridge-utils uml-utilities
{% endcodeblock %}

Change these two files.
{% asset_img 1.PNG %} 
{% asset_img 2.PNG %} 
 
One thing to note here. The enp0s3 is the interface on my system. You need to change it to your interface’s name otherwise it won’t work.

You can restart the system now to make the changes take effect. Then we are going to download the qemu mirror. We can get it from here.

{% asset_img 3.PNG %} 

I download the <b><i>Debian_squeeze_mips_standard.qcow2</i></b> and the <b><i>vmlinux-2.6.32-5-4kc-malta</i></b>.

After we finish this, we can use this command to start the qemu.

{% codeblock lang:bash %}
Sudo qemu-system-mips -M malta -kernel vmlinux-2.6.32-5-4kc-malta -had Debian_squeeze_mips_standard.qcow2 -append “root=/dev/sda1 console=tty0” -net nic, macaddr=[your interface’s mac] -net tap
{% endcodeblock %}

{% asset_img 4.PNG %} 

Use root/root to login. Then you may realize that your interface here is eth1 so change the <b><i>/etc/network/interfaces</i></b> file. Make it like this. 

{% asset_img 5.PNG %} 

You can use ctrl+alt to escape the window. Restart the virtual machine here and you can use ssh to login the qemu to make your life easier.

{% asset_img 6.PNG %} 

Any question, email me at xudong_shao@hotmail.com