---
title: 'Unpacking: Delete the .reloc section'
date: 2018-03-11 11:31:42
tags:
- 2018
- Unpacking
---

Today, let’s do something interesting. We all know that <b><i>.reloc</i></b> section is used for relocation in the PE file and it is essential for most DLL file because usually these files can’t be loaded into the desired image base. But for the PE file, it might not be so important. So, what we will do is to delete this section and change some bytes in the PE file to make it runnable. By doing this, hope we can have a better understanding of the PE format. Let’s start.

I used this C++ code to build the file.

{% codeblock lang:C++ %}
#include<iostream>

int main(){
    std::cout<< “Delete the .reloc section!” << std::endl;
    system(“pause”);
}
{% endcodeblock %}

First, we need to delete the .reloc section header. Use the PEview we can see this.

{% asset_img 1.PNG %}

This section header starts at offset <b>300</b> and the size is 28. So overwrite this part with 0 using HxD.

{% asset_img 2.PNG %}

We can also see from PEview that the .reloc section starts at offset <b>EC00</b> so let’s delete all the data starts from that place.

{% asset_img 3.PNG %}

Don’t celebrate now, there are other things we need to do.
In the <b><i>IMAGE_FILE_HEADER</i></b> structure, there is a data for <b>Number of Sections</b>. In our case, we can see it’s 8.
 
{% asset_img 4.PNG %}

Since we deleted one section, we should decrease it to 7.
 
{% asset_img 5.PNG %}

And we need to change the <b><i>Size of Image</i></b> in the <b><i>IMAGE_OPTIONAL_HEADER</i></b>.

{% asset_img 6.PNG %}

We know the virtual size of the .reloc section is 771 and due to the section alignment we should make it as 1000. In this case, the new Size of the image will be 24000-1000=23000.

{% asset_img 7.PNG %}

Ok, let’s run the file.
 
{% asset_img 8.PNG %}

It's done!

Any question, please contact me at <b><i>xudong_shao@hotmail.com</i></b>.