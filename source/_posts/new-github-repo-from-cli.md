title: New Github repo from cli
date: 2015-11-01 15:23:21
tags: [github, cli, fast-coding]
thumbnail_image: github.png

---

Since I found myself having to create a lot of github repos I wrote this simple bash function. 

{% gist a2167f3e33895148dbba github.sh %}

So, you just have to add this code to your **.bashrc**/**.zshrc** or to run it as bash script. 

Also, this script can be installed using bpkg: 

```bash
bpkg install nemanjan00/githubrepo
```

For GitHub authentication, you need to create **~/.github** file with content like this: 

{% gist a2167f3e33895148dbba .github.example %}

To execute script, run: 

```bash
githubrepo nameofrepo
```

This will create repo with name **nameofrepo** and add it as **origin** to current git tree. 

