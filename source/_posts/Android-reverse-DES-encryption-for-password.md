---
title: 'Android reverse: DES encryption for password'
date: 2017-12-26 18:08:47
tags:
- 2017
- Android reverse
---

Today, we will analyze an app which uses DES in CBC mode to encrypt the password and sends it to the server. Ok, let’s start.
First, we tried to login in and Fiddler caught the packet.
 
{% asset_img 1.PNG %}

Let’s check that webform.
 
{% asset_img 2.PNG %}

Yeah. Today’s topic will be this password. And we can open the apk in disassembly tool. I am using android killer which is a Chinese software but you can use JEB which should work well in this case. After that you can just search for <b><i>“user-login”</i></b> in the tool. This is the key word in the post url. And through a series of process we can find the native library which contains the encryption algorithm.
  
{% asset_img 3.PNG %}
{% asset_img 4.PNG %}

Ok, we can see that the native library is libm4399.so. We can open it in the IDA.

If the library has an export function for <b><i>DesCbcDecrypt</i></b>, the function name should begin with JAVA but there isn’t one like this. So the function should be dynamically loaded. We have to find that function in <b><i>Jni_Onload</i></b>.
 
{% asset_img 5.PNG %} 

And if you want load the jni header, you will find that the <b><i>off_11058</i></b> is the 3 element tuple which points to that function we need. After several double clicks, we will come to the final encryption function. And at the beginning you will find it get the input string here.
 
{% asset_img 6.PNG %}

If you have an android phone you can just debug the app and  find that password in the memory at this place. Because we can see that it load the result into the R0.

{% asset_img 7.PNG %}

Then it get the length of the password and allocate some memory on the stack. Just step forward, you will get to the DES encryption finally.
 
{% asset_img 8.PNG %}

Double click and get there.
 
{% asset_img 9.PNG %}

Well, actually if we debug that on the phone and the code should be a little different here which we should see the Initial vector and the key for the DES. But here we can’t see it. But we almost know how it encrypt that password. There is one more thing. It actually not just encrypt that password. There is a padding. Check it out.

{% asset_img 10.PNG %}  

In the code marked 1, we see that it calculates the padding of the password. And in the code marked 2, it calculates the length of the padding.

So, that’s all for it today. Any question, please contact me at <b><i>xudong_shao@hotmail.com</i></b>
