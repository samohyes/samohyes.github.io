---
title: 'Unpacking: 3 interesting unpacking cases'
date: 2018-03-24 15:42:26
tags:
- 2018
- Unpacking
---
I want to introduce some interesting unpacking cases that I encountered these days. And you can find some video tutorials for these from MalwareAnalysisForHedgehogs and hasherezade on youtbe.


<b><i>Case 1</i></b>

TOOLS: python, HxD
Sample: https://www.hybrid-analysis.com/sample/a6cbeeecfb50bf94499547fbf0adc6451326be45a16462f9056eaba2342fcf2c?environmentId=100

We know some packing techniques are that they use a key to ‘XOR’ with the original PE file so and embed it into a new one. In this case, we can see that if we know the key we can easily unpack it. This is the method we are going to use.

Firstly, open the file in the HxD.

{% asset_img 1.PNG %}

We can see the repeating string <b><i>”W..1..4..3..5..8.....”</b></i>. For now, we will try this string as the key to see what we get. And the reason why we think the original PE file starts at the highlighted place is that the second byte in the key is 0 and “Z XOR 0 is Z” so that might be the start of MZ. So copy from that part.

{% asset_img 2.PNG %}

Then we will write a python script to unpack the payload.

{% asset_img 3.PNG %}

After running our code, we have our payload now.

{% asset_img 4.PNG %} 


<b><i>Case 2</i></b>

TOOLS: PE-sieve 
Sample: https://www.hybrid-analysis.com/sample/008c4bd6ee834d113cfc693af0ea90396eaa47e860bcdd567ffd964b57434e1d?environmentId=100

Basically, pe-sieve is a tool developed by hasherezade and it can actually dump the PE file in the memory. And it can deal with some custom packers. But for this sample, it is not that easy.

Run the sample and get the PID. Then use the pe-sieve to dump the payload.

{% asset_img 5.PNG %}

But if we open the exe and dll file we get in pe-studio we may find out that they are not the payload. And the exe file seems having another loader. So run that file again. Suspend it. Open the OllyDbg and attach to the new process.

{% asset_img 6.PNG %}

And check the memory you will find there is a very interesting section.

{% asset_img 7.PNG %}

This section seems like a PE file without the MZ and PE magic number. Because we can see all those section names there. So what we are going to do is to add the magic number to the file.

{% asset_img 8.PNG %}

Then the pe-sieve can dump the payload. You need to use the command “pe-sieve PID /imp” to automatically recovery the import directory.

{% asset_img 9.PNG %}

And we can see that the payload starting at 220000 has been successfully dumped and there are many suspicious APIs.


<b><i>Case 3</i></b>

TOOLS: OD 
Sample: https://www.hybrid-analysis.com/sample/482a8b7ead1e07ac728e1e2b9bcf90a26af9b98b15969a3786834d6e81d393cd?environmentId=100

We know sometimes some scripts are wrapped in the .exe file. For this sample, it is a wrapped .bat file. You can also use DIE to check it.

{% asset_img 10.PNG %} 

For this one, the key point is to set a breakpoint on <b><i>WriteFile</i></b>. Because the malware will finally write the payload into the disk.

{% asset_img 11.PNG %}

Copy this part using a hex editor. 

{% asset_img 12.PNG %} 

Then you can change the extension to .bat and check the payload.

{% asset_img 13.PNG %} 
