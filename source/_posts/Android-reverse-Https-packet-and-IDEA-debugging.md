---
title: 'Android reverse: Https packet and IDEA debugging'
date: 2017-12-27 23:00:38
tags:
- 2017
- Android reverse
---
So today we will try to decode the https packet and debug with IDEA.

<b><i><u>HTTPS packet<b><i><u>

You need to redirect the traffic through the fiddler. So the key point here is you can install the fiddler’s certificate and then you can decode those https packet.

{% asset_img 1.PNG %}

Click here to install the certificate. After that, you are able to see those Https traffic now.

<b><i><u>IDEA debugging<b><i><u>

For <b><i>IDEA</i></b> debugging, make sure you install the smalidea plugin. Install the apk in the emulator and start it in the adb. Let’s say you want to debug it at the beginning of the program.

{% asset_img 2.PNG %}

The app’s name should be <b><i>com.sfbest.mapp</i></b>. And the module name is <b><i>module/guide/GuideActivity</i></b>. You can start it in the adb like this.
 
{% asset_img 3.PNG %}

Now, import the project to IDEA. Make sure you disassemble the apk so you know the path of the smali file. Also make sure that you add this piece of code in the .xml file otherwise you won’t be able to debug the app.

{% asset_img 4.PNG %}

Then open the ddms.

{% asset_img 5.PNG %}

See that 8700 port. 

{% asset_img 6.PNG %}

Go to <b><i>run-> Edit configuration</i></b> and set the port to 8700. And just run it. You can set breakpoints and see those variables.

{% asset_img 7.PNG %} 

So yeah, any question please contact me <b><i>xudong_shao@hotmail.com</i></b>
