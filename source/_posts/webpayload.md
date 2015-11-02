title: webpayload
date: 2015-11-02 16:33:34
tags:security
---

[webpayload](https://github.com/nemanjan00/webpayload) is something pentesting community have missed out for some time now. Since I had to do some pentesting on my own servers, I decided to write this "patch". It is just a C wrapper for a metasploit payload (payload/linux/x64/shell_reverse_tcp) providing Dynamic DNS support. 

Basic idea is to create fast and simple reusable payload whih can be packed inside C applications, C++ applications, PHP...

There is also simple (for now) example of how it can be delivered inside PHP as binary or source. You can pack binary inside PHP and just execute it on server. Or, for more powerfull solution, you can pack source inside PHP and compile it on server side. 

{% gist 24bd8ec7553d4c76044e backdoor.c %}

This is how C code looks like.  As you can see it is being populated with setting before compilation. 

On execution, it finds IP using DNS and inject it into Shellcode and then it does stack overflow to execute shellcode. 

For now PHP is there just to drop files and to run gcc.

Future idea is to implement more flexible PHP in the area of disabled functions and to implement functio which will find dir where PHP can write.

Also I plan to encrypt data to make it more difficult for Antiviruses to detect it. 

