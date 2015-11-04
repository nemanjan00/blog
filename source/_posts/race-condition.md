title: race condition
date: 2015-11-03 10:10:38
tags:

coverImage: race-condition.png

---

## The problem

Race condition is very common problem which is not mentioned so commonly and that is not so good. 

Problem lays in pretty simple fact. Code is not executed in real-time. 

Let's say we have web store. When customer buys something, our code first checks if customer have enough money. After that, if customer have enough	money, code checks if that item is currently available in store. If it is, then it takes amount it got while checking amount first time, subtract it with item price and update users credit. 

```
[QUERY USERS CREDIT]
[CHECK IF CREDIT > COST]
[CHECK IF ITEM IS AVAILABLE]
[SUBTRACT USERS CREDIT FROM STEP 1]
[UPDATE USERS CREDIT]
```

At first, it all looks cool... But, wait a minute! What if user send two requests one after another? 

```
[QUERY USERS CREDIT]
			[QUERY USERS CREDIT]
			[CHECK IF CREDIT > COST]
			[CHECK IF ITEM IS AVAILABLE]
			[SUBTRACT USERS CREDIT FROM STEP 1]
			[UPDATE USERS CREDIT]
[CHECK IF CREDIT > COST]
[CHECK IF ITEM IS AVAILABLE]
[SUBTRACT USERS CREDIT FROM STEP 1]
[UPDATE USERS CREDIT]
```

So, user just got two items for price of one. Not what we wanted, right? 

## Real-life demonstration

Just to prove how big of a problem this is, I created very simple demo. 

This is file which is vulnerable: 

{% gist 98f974dca5167690f456 test.php %}

This is file we are executing to exploit vulnerability: 

{% gist 98f974dca5167690f456 index.php %}

So, this is how it looks like. Instead of 100, it outputs 58.

```bash
╭─nemanjan00@nemanjan00-laptop  ~/race-condition ‹› ‹master› 
╰─$ cat counter.txt 
0
╭─nemanjan00@nemanjan00-laptop  ~/race-condition ‹› ‹master› 
╰─$ php index.php 
╭─nemanjan00@nemanjan00-laptop  ~/race-condition ‹› ‹master*› 
╰─$ cat counter.txt
58 
```

## So, what is the solution?

If you are working with file, there is very simple solution. It is called [flock](http://php.net/manual/en/function.flock.php). Basically, you lock file while you work with it and scripts works with it one by one. 

{% gist 98f974dca5167690f456 test_secure.php %}

Here is example executed: 

```
╭─nemanjan00@nemanjan00-laptop  ~/race-condition ‹› ‹master*› 
╰─$ cat counter.txt     
0
╭─nemanjan00@nemanjan00-laptop  ~/race-condition ‹› ‹master*› 
╰─$ php index_secure.php
╭─nemanjan00@nemanjan00-laptop  ~/race-condition ‹› ‹master*› 
╰─$ cat counter.txt     
100
```

For database it is pretty similar process so I will not be describing it. If you need to find how to do it in other case, just google it. 
