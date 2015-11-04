title: race condition
date: 2015-11-03 10:10:38
tags:
---

Race condition is very common problem which is not mentioned so commonly and that is not so good. 

Problem lays in pretty simple fact. Code is not executed in real-time. 

Let's say we have web store. When customer buys something, our code first checks if customer have enough money. After that, if customer have enough	money, code checks if that item is currently available in store. If it is, then it takes amount it got while checking amount first time, subtract it with item price and update users credit. 

[QUERY USERS CREDIT]
[CHECK IF CREDIT > COST]
[CHECK IF ITEM IS AVAILABLE]
[SUBTRACT USERS CREDIT FROM STEP 1]
[UPDATE USERS CREDIT]

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

