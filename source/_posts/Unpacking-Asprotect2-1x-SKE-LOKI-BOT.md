---
title: 'Unpacking: Asprotect2.1x SKE(LOKI BOT)'
date: 2018-01-17 13:41:47
tags:
- 2018
- Unpacking
---

I downloaded a malware from here.

http://www.malware-traffic-analysis.net/2018/01/08/index2.html

PEID says it’s packed with Asprotect2.1x SKE.

 {% asset_img 1.PNG %}

Well, I tried two hours to unpack it without an unpacker but failed. So I finally used an unpacker. It is from here.

https://tuts4you.com/e107_plugins/download/download.php?view.3519

I feel like taking a free bus. But yeah, maybe I will learn more and unpack it without an unpacker.

So open the malware in Ollydbg and run the script we got. So you can see the information it provides in the <b><i>logdata</i></b> view.

 {% asset_img 2.PNG %} 

We can see the RVA of OEP here. So open the <b>ImportREC</b>. 
 
 {% asset_img 3.PNG %}

Click the <b><i>IAT AutoSearch</i></b> and <b><i>GET Import</i></b> button, then fix the dump. After this, you can see it’s unpacked.
 
 {% asset_img 4.PNG %}

Any question, please contact me at <b><i>xudong_shao@hotmail.com</i></b>.

