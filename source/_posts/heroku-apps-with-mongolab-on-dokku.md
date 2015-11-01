title: Heroku apps with MongoLab on Dokku
date: 2015-11-01 13:07:43
tags: [heroku2dokku, heroku, dokku]
---

If you want to install Heroku app with MongoLab to Dokku, you can do that really easly. 

1) Install dokku-mongo plugin


```bash
dokku plugin:install https://github.com/dokku/dokku-mongo.git mongo
```

2) Create database

```bash
dokku mongo:create databasename
```

3) Link database to app

```bash
dokku mongo:link databasename appname
```

4) Set MONGOLAB_URI

Since Heroku uses MONGOLAB_URI and dokku-mongo sets MONGO_URL, you just need to set MONGOLAB_URI

```bash
dokku config:set appname MONGOLAB_URI=$(dokku config:get appname MONGO_URL)
```

After that, it should work. 

