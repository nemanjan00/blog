title: Debugging Atom editor and Arch problem
date: 2017-10-17 19:57:42
tags:
---

So, I just had a problem starting Atom editor. 

I do not run it too often (mostly for search of projects). 

For some reason, it did not want to start. 

So, I wanted to debbug it. 

## How to debbug Atom? 

When I start it, it just goes to background... 

After a little of investigation (``cat $(which atom)``) I figured out output was piped into ``~/.atom/nohup.out``...

## What was the problem? 

There was error in output and it said that module inside atom was built with different version of Node. 

From my experience, with packaged applications, that most often means it was ran by wrong version of electron. 

I also have electron installed globally and that version is default. 

## Fix? 

Well, I now need to make it run with electron installed by package manager... 

First I listed files of that package: 

```bash
pacman -Ql electron | grep bin
```

I got ``/usr/bin/electron`` as a response. 

Now, I found command that starts atom in ``cat $(which atom)`` and just replaced electron with full path of one from package manager. 

``` bash
/usr/bin/electro --app=/usr/lib/atom
```

It worked as expected so, now I just created alias for atom... 

```bash
alias atom="/usr/bin/electron  --app=/usr/lib/atom"
```

And that was it. 

