title: reportr on dokku
date: 2015-11-01 12:49:26
tags: [heroku2dokku, heroku, dokku]
---

Recently, I have found reportr and, since I use dokku, I decided to install it inside my dokku environment. 

Since I am lazy, first step was to just deploy app and figure out errors one by one. 

```bash
git clone git@github.com:Reportr/dashboard.git
cd ./dashboard

git remote add dokku dokku@dokku:reportr
git push dokku master
```

1) Swap

Since my server did not have swap, I encountered this problem. If your does have swap, you will not encounter this one. 
To solve this one, just create swap. 

2) MongoDB problem

Solution: 
``` bash
dokku plugin:install https://github.com/dokku/dokku-mongo.git mongo

dokku mongo:create reportr
dokku mongo:link reportr reportr
```

3) MONGOLAB_URI

Since, on heroku, URL for MongoDB is MONGOLAB_URI instead of MONGO_URL I had to create new environment variable. 

```bash
dokku config:set reportr MONGOLAB_URI=$(dokku config:get reportr MONGO_URL)
```

After that, just set AUTH_USERNAME and AUTH_PASSWORD and you are ready to go. 

Now, it all works... 

