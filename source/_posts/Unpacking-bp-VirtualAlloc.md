---
title: 'Unpacking: bp VirtualAlloc'
date: 2017-12-10 21:28:14
tags:
- 2017
- Unpacking
---
I decide to include some unpacking posts in my personal blog. So here it is.

VirtualAlloc is the api that malware use to allocate memory for the unpacking process so we can use it to unpack malware. I’ve found some tutorials for this strategy but they are not suitable for the sample here. You can check it yourself.

{% link unpacking-using-ollydbg http://paulslaboratory.blogspot.com/2014/04/unpacking-using-ollydbg.html %}

Here is a video using the same method to unpack a malware.
{% youtube BCis7FwffiU %}

So this is the beginning of the unpacking.

{% asset_img 1.png %}

First, we input “bp VirtualAlloc” in the command input here.

{% asset_img 2.png %}

We can see now there is a breakpoint.

{% asset_img 3.png %} 

Then you press F9 to run it. You come to here.

{% asset_img 4.png %}

Here is the code in DLL. Now you use “ALT+F9” to come back to the user code. Then you have to press F8 to continue going and you will finally find a “jmp” which lead you to the OEP.
 
{% asset_img 5.png %}

Actually, for this sample, using ESP should be much easier. But we can see even we are using the VirtualAlloc, there are still some difference for different samples. Those links that I provide can verify this. So always be patient when you are unpacking a malware.
