---
title: 'Crackme: 2'
date: 2017-12-21 14:42:23
tags:
- Crackme
- 2017
---
This is the second post for CrackMe! Today’s sample is easy too.

{% asset_img 1.PNG %} 

User needs to input the name and serial. There is one thing to mention here that if you open this sample with IDA you probably can’t change it into graph view so here I only use Ollybdg to crack it.

Open Ollydbg and search in the string view you will find this.

{% asset_img 2.PNG %}

So double click the <b><i>“You Get It”</i></b>, you will come to the important function here.

{% asset_img 3.PNG %} 

You have to be patient now and go up to see the code. And you come across this piece of code.

{% asset_img 4.PNG %}

It seems that the functions use strcat to link the <b><i>“AKA-”</i></b> and something else. So go up.

{% asset_img 5.PNG %}

Ok, this is the algorithm to generate that key. First, you have this vbaLenBstr to get the length of the name. Then multiply it by <b><i>0x17CF8</i></b>. In the <b><i>0x40242D</i></b>, it gets the ascii value for the first character of input name. Add that value to the previous multiply result. Finally, change the Hex into Decimal which is the function <b><i>“vbaStrI4”</i></b>.

Let’s write a simple key generator in python.

{% asset_img 6.PNG %}
