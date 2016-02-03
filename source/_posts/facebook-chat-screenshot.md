title: Facebook chat screenshot
date: 2016-02-03 20:55:58
tags:

coverImage: facebook.jpg
thumbnailImage: facebook.jpg

---

I think a lot of people at one point needed screenshot of whole facebook conversation. So, I decided to develop firefox add-on to complete that task.

## Idea

First of all, I needed to find a way to approach problem. Since facebook just has too much of JS and CSS for me to read and analyze I decided to use hackerish way of thinking. 

Basically, a lot of plugins today support screenshot of entire page, but there is problem with facebook. Chat is inside scrollable frame. So, these plugins can't take picture of whole chat. 

What I did is find "markers" in DOM and by using them I was able to reference proper elements and resize frame so whole chat is visible. 

## Demo

{% youtube go8Qw9A9Z-Y %}

## Plugin download

You can download it on Firefox Add-On market. 

[Full FB chat](https://addons.mozilla.org/en-US/firefox/addon/full-fb-chat/)

## Plugin I used dor screenshot

[https://addons.mozilla.org/en-US/firefox/addon/nimbus-screenshot/](Nimbus Screen Capture)

