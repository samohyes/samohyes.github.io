---
title: 'Unpacking: using HxD'
date: 2017-12-14 18:25:53
tags:
- 2017
- Unpacking
---
Today, let’s unpack a malware named <b><i>Fleercivet</i></b>. Before we start, I have to say that the <b><i>MalwareAnalysisForHegehogs</i></b> has already done a quite good video tutorial for this sample, you can check it out here.

{% youtube BCis7FwffiU %}

So, let’t see how we can unpack it using a different way.

The first part is unpacking. In the video they did quite well using the breakpoint at <b><i>VirtualAlloc</i></b> and I also write a post on this method so you can check it out too.

http://xdshao.com/2017/12/10/Unpacking-bp-VirtualAlloc/

But for this sample, you can see that the speaker used a plugin to dump the memory and unpack it. To be honest, I can’t find the plugin and the link they provide seems not working anymore. So here, I use another way to achieve the same purpose.

Let’s say you follow the video and come to this step where you see at the address 0X3A0000 you find “MZ” which is the start of the PE format. To be more specific, it’s the start of the DOS header.

{% asset_img 1.png %}

So you open the HxD and attach it to the process in the memory which is the sample name. And then you jump to the 0x3A0000.

{% asset_img 2.png %}

And you can also find that, the content is in the range of 0x3A0000 to 0x3D0FFF. Because there is some meaningless things starting at 0x3D1000.
 
{% asset_img 3.png %}

So all you need to do is to copy the content between 0x3A0000 and 0x3D0FFF to a new file then save it. You can use the 	PortexAnalyzer.jar to check the SHA256 and it’s the same as that in the video.
 
{% asset_img 4.png %}

Any question, please contact me at xudong_shao@hotmail.com
