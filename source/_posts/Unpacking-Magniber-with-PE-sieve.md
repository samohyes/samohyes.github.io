---
title: 'Unpacking: Magniber with PE-sieve'
date: 2017-12-27 00:49:00
tags:
- 2017
- Unpacking
---
So today we will be unpacking a Magniber ransomware with PE-sieve. This tool is developed by <b><i>hasherezade</i></b>. She actually did a very good tutorial here.

{% youtube lqWJaaofNf4 %}

The tool can find the hooks and dump the payload at that point. Also it can be used to dump process hollowing. Actually you can check the comments below that video and you will see me posted some questions there and <b><i>hasherezade</i></b> replied me and instructed me with great generosity. So today’s post will mainly repeat the work she did in that video. 
 
{% asset_img 1.PNG %}

Here we just unpack it with upx. Then open that with Ollydbg. 
You have to change a bit in the setting of debugging option to make it stop at the entry point of new modules. Then Press F9 until you find that the sample create a new child process.
 
{% asset_img 2.PNG %}

And yeah, right now, use the <b><i>pe-sieve</i></b> to dump the payload. So why should we dump it at this specific point, that’s because the malware will use <b><i>cmd</i></b> to delete itself right now and it has already install the hook.
 
{% asset_img 3.PNG %}

Yeah, now we are done here! If you try to open the sample.exe which is running right now in the HxD, you will find exactly the same binary with the file we dumped.

{% asset_img 4.PNG %}

Well, <b><i>hasherezade</i></b> helped me with unpacking a process hollowing malware. Her video is here.

{%youtube 7xtxOD1LX7U %}

And I tried using her way and compared it with the thing that I dumped with my original way here.

<a href="http://xdshao.com/2017/12/20/Malware-analysis-Dridex-and-process-hollowing/">Malware analysis: Dridex and process hollowing</a>

{% asset_img 5.PNG %}

The results are nearly the same. Only few bits different here and the rest is identical. This seems reasonable to me.

Any question, please contact me at <b><i>xudong_shao@hotmail.com</i></b>
