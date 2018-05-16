---
title: 'Unpacking: a cryptocurrency miner'
date: 2017-12-25 15:29:11
tags:
- 2017
- Unpacking
---
Today, we will try to unpack a cryptocurrency miner one section by one section. And actually here is a very good tutorial. I almost follow its instructions.

{% youtube bVCPbABtKo8 %}

First, set breakpoint on <b><i>WriteProcessMemory</i></b>.
When the first <b><i>“WriteProcessMemory”</i></b> hit, dump the PE in the memory.

{% asset_img 1.PNG %}

You have to delete the trash before the <b><i>“MZ”</i></b> to make it a legitimate PE format. Then run again, dump the PE in memory to see if there is any difference between the 1st and 2nd edition. Actually they are just the same file.

{% asset_img 2.PNG %}

So run again, dump again.

{% asset_img 3.PNG %}

This time, the 2nd and 3rd PE file are different in the <b><i>.text</i></b> section. So let’s save that <b><i>.text</i></b> section aside. Run again and dump the 4th edition.

{% asset_img 4.PNG %}

This time, we can see that the <b><i>.text</i></b> section is actually filled with 00 in 4th one. And its <b><i>.rdata</i></b> section is different with the 3rd one while the other two sections are the same. So just save the <b><i>.rdata</i></b> section.
Run again.

{% asset_img 5.PNG %}

This time the <b><i>.data</i></b> section changed. So save that section. The last one changed the <b><i>.reloc</i></b> section.

{% asset_img 6.PNG %}

So we have the 6th edition. Because the malware only write one section a time. So for the 6th editon, it only has a valid section which is the <b><i>.reloc</i></b> section. Remember we save some sections before? Yeah, all you need to do is to substitute other sections in 6th edition with those sections we saved before.  

{% asset_img 7.PNG %}

Then you get an unpacked PE. Just save that.
Ok, the malware might use process injection and typically might be process hollowing to start a child process of its own and replace that. You can have a check with my post about this.

{% asset_img 8.PNG %}

So here we can use the Scylla to dump that child process out.
But to be honest, I dumped that child process and compare it with the one that we filled with each section. They are different. The one I dumped is different and there must be something wrong with my dump method. I just don’t know why after many hours’ trying.

Yeah, that’s it. 

Any question, please contact me at <b><i>xudong_shao@hotmail.com</i></b>
