title: Dokku
date: 2017-10-19 21:55:15
tags:
---

Dokku is extremly simple PaaS (platform as a service) . 
Philosophy of this software is quite simple. It is as simple as possible clone of Heroku. 

Advantage of system like this is that you do not have to (but you can) think about container application is going to run inside but only about application itself. All you need to do is git push and Dokku will make fully functional container out of it and deploy it. 

## What happends when I do git push? 

First thing you need to know about Dokku, Heroku and similar software is that they use something called buildpacks. Buildpack is collection of additional software required to build and run your application. For example there is PHP buildpack that contains PHP, ngginx, apache, composer, etc. 

---

### What is buildpack and how do we use it? 

Buildpack is collection of 3 scripts with specific role. 

* bin/detect - Role of this script is to detect if that buildpack was developed to be used with application like yours. This one is skiped if user already defined buildpack and is used for autodetect of language only. 

* bin/compile - Role of this script is to prepare your application for running (installing dependencies, building, minification or stuff like that)

* bin/release - Role of this script is to tell which commands should you run to start this container. 

---

As you have probbably already figured out, first we need to figure out which buildpack should be used for your application. 

It can be either specified by user or detected by Dokku/Heroku/etc. by running all buildpacks against script until detect script returns true. 

After that, compile script is run to prepare your application for deployment, 

After that, for Dokku to be able to know how to run container, release return list of commands. 

Example:

```
web: node index.js
```

If your application does not have Procfile, the one from release script will be used. 

There is just one step left after this... Image is commited and ran. 

The only remaining whing is communication between your application and outside world. For anyone to be able to connect to your application from outside, your application needs to listen on specific port. 
For default http port, dokku is setting global variable called PORT to port number. (And nginx is used to proxy to your application)

You can also configure Dokku to forward other ports to your application. 

#### What are advantages of system like this? 

There are a lot advantages but thrse are the ones that got me to switch: 

- Consistent enviroment - You are sure your application will act the same way on any server that uses Dokku. 

- Easy deployment and fast first time configuration

- There is big community and a lot of plugins for it. 

