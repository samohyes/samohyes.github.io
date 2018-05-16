---
title: 'Android reverse: login traffic analysis'
date: 2017-12-22 00:19:21
tags:
- 2017
- Android reverse
---

I will include some posts for android reverse and security too since this is the area that I’ve been learning and have great interest in. But to mention first, android reverse is really not friendly to newbies. I’ve spent 1-2 month to get myself familiar to android programming and JAVA. This is far from getting to the door of android reverse. These days, I am reading some books and watching videos of android reverse. But I just decide to write something to help me understand those materials. So here it is. We will analyze a app’s login traffic.

First, we need to install the apk in the emulator and open the fiddler. Then you have to configure the WLAN setting in the emulator to direct the traffic to the fiddler. There are many tutorials for this. For example, you can use Genymotion to do this. But here, I am using a Chinese emulator.

So, first open the app and use <b><i>“samohyes”</i></b> and <b><i>“123456”</i></b> to login and in fiddler you will find this packet.

{% asset_img 1.PNG %}

Yeah, we catch that traffic. Then let’s see what’s inside that packet. Use the raw tab in the fiddler and view the packet in the notepad.
 
{% asset_img 2.PNG %}

So seems the body of the packet is some kind of hashing. But we are not sure what is it exactly. So let’s disassemble it and have a deep look in it. Open this apk in JEB. Since in the packet we see <b><i>“cmd=mobileUser.login”</i></b> so we can try to search this string with <b><i>“CTRL+F”</i></b> and this will leads us to a class <b><i>Lcom/a/a/a/b/s</i></b>. And when we scroll down we can see a JSON and this concludes the thing we input and the app will encrypt it and submit it.

{% asset_img 3.PNG %}

Here the most important function will be this <b><i>com.a.a.a.f.a.a</i></b> which encrypt the input so we double click it and see where we head to.
Here is a <b><i>“encryptString”</i></b> method and double click it again. Ok, then we come to here. 

{% asset_img 4.PNG %}

For the this.ecode(), you will see this is probably change the input to HEX value. And then, the key point is that <b><i>getEncryptionString</i></b>. If you double click it, you will find it’s a native founction. To analyze this function, we have to use IDA to analyze that .so file. And actually what it’s doing is using a hard-coded key and DES to encrypt that information. But we are not going to analyze native codes in this post. Instead I’d love to show you how we debug the apk with smali. 
So in JEB we decide to debug in this function.
 
{% asset_img 5.PNG %}

We can put a debug output before and after that <b><i>encryptString()</i></b> function. To get the debug information in DDMS. I used a smali file that help me to get the output and all we need is to put that smali under the same directory with the com project. And I add these two codes. 

{% asset_img 6.PNG %}

Then we run it and get the outputs in DDMS.

{% asset_img 7.PNG %} 

Any question, please contact me at <b><i>xudong_shao@hotmail.com</i></b>
