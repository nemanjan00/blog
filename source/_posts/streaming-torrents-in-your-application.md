title: Streaming torrents in your application
date: 2017-10-21 21:06:41
tags:
---

Hi guys, 

I created a few applications that stream torrents, so, I get asked quite a bit about it. 

Today I am going to write a little bit about how it works and how to utilize it. 

## Where do I get torrents from? 

First of all, to get torrents, you can use search engine/file someone sent you/create your own torrent... 

So, what torrent file/megnet link is? 

## Torrent file/magnet link

When you get torrent in any form, what you actually get is hash of that torrent. 

It can be either looked up on Torrent trackers (web services) or DHT (distributed hash table). 

The part that is actually looked up is hash of that torrent. 

That hash is used to identify and verify that data integrity. 

## What do we do with data from tracker/DHT? 

What we get from DHT is list of peers that have torrent we are looking for. 

What happens next is we download data about torrent from those peers. (File list, sizes, etc.)

## Ok, how do we download files now? 

What is happening now is we are reaching out to peers and they are offering parts they have. 

For streaming, we are simply accepting parts that are near area we need and deny those that we do not need at the momment. 

## How can we implement it? 

If you are writing your application in Node.JS, what I would recommend to you is [torrent-stream](https://github.com/mafintosh/torrent-stream) package. It has very simple API and it is very easy to pass it through HTTP layer. 

If you are writing your application in any other language, I would recommend using [peerflix](https://github.com/mafintosh/peerflix) which is CLI version of torrent-stream. 

