---
title: 'Crackme: 1'
date: 2017-12-19 19:56:06
tags:
- 2017
- Crackme
---

I’ve got more time during winter holiday so I will also do some crackme problems.

Today's crackme problem is like this.

{% asset_img 1.png %}

You need to input a name and a serial here. Try <b><i>“sam”</i></b> and <b><i>“123456”</i></b> you will fail let this.
 
{% asset_img 2.png %}

OK, let’s open it in IDA. In the string subview you can find a string <b><i>“Good job dude=)”</i></b>. Double click it and you will come those important functions.
The first check is here.

{% asset_img 3.png %}

It checks whether your name is longer than 3. Your name needs to have at least 4 characters. And in <b><i>0x42FA8A</i></b> you will find that you put the first character into eax and multiply it by 29H.
 
{% asset_img 4.png %}

For “s” you will get 24D6.

And in <b><i>0x42FACD</i></b> you will find it generates the key.

{% asset_img 5.png %} 

Feel free to go into that function and see how it turn 24D6 into 9430. But actually it just change the hex value into decimal. Yeah, to be honest, I went into that function and spent a lot of time to analyze it and finally, I see that 24D6 and 9430 are the same number… This is sad.

Ok, so you will get the key which is <b><i>“CW”+ ”the key you generate”+ ”CRACKED”</i></b>.

Any question, please contact  me at <b><i>xudong_shao@hotmail.com</i></b>
