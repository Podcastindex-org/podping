# Podping
Podping is a global message bus for podcast infrastructure events.  Entities that publish feeds can "write" events into the
system and those events are then visible to all parties who "watch" for them.  Anyone can watch/monitor for events and
respond appropriately, depending on their needs.  It was developed as an alternative to WebSub and rssCloud which both
have major drawbacks when it comes to podcasting infrastructure.

<br>

## Problem

WebSub is a wonderful technology for the broader RSS world, and for small subscription bases. But, there are four main 
issues that make it less than ideal for the open world of podcasting:

1. Not all hosting companies have chosen to support it.
2. The burden of resubscribing on a per-feed basis every 7-15 days goes up exponentially as the feed count grows into 
   6 or 7 digits.
3. The WebSub ecosystem of hubs consists mainly of SuperFeedr and Google. Most feeds are concentrated on those two.
4. WebSub uses web hooks, so it requires having a server in order to receive notifications.  This isn't useful for 
   standalone apps.

A final issue is that of reliability. In our experience, hubs have proven to be unreliable at times. Especially the 
free ones. We have seen what appear to be outage periods, or just silence.

<br>

## Solution

A better solution than subscribing and re-subscribing constantly to individual podcast feeds is for aggregators, 
directories and apps to just subscribe to a single firehose of all podcast feed urls that publish a new episode. 
Podcast publishers notify [Podping.cloud](https://github.com/Podcastindex-org/podping.cloud) that a url they manage 
has updated. The podping server validates that publisher's identity and then writes the feed url, and any associated
"reason" and "medium" codes to the [Hive](https://hive.io/) blockchain.  In the future, other blockchains and pubsub
protocols will be supported.

Hive is an open blockchain that writes a new block every 3 seconds, and accepts custom data written into it. We simply 
write the urls of the updated feeds as a custom JSON object into the blockchain as we receive them. This allows anyone 
to just "watch" the Hive blockchain and see within about 20 seconds that a podcast feed url has updated and be 
confident that it's the Hosting company (or self-hosted podcaster) themselves that sent that notification.

Multiple podping servers in different global regions will be run to ensure that there is no single point of failure. 
The "podping.cloud" host name will be load balanced to these servers. Any of the servers can handle receiving and 
writing urls to the blockchain.

The bottom line is that podcasters and hosting companies just send a GET request with the url to a single web address, 
and everyone else in the industry can see that update within a few seconds with no subscribing (or re-subscribing) 
required by anyone.

<br>

## Rollout

Initially, podping will be run by Podcastindex.org to facilitate its beta testing and development. Podcast Index will 
run multiple servers to distribute load and provide redundancy. But, we are building this software as an open source 
project so that it can eventually be run quickly and easily by any hosting provider, or even baked into other 
podcasting CMS projects.

We encourage anyone and everyone to contribute to its development.

<br>

## The Parts
The Podping system consists of multiple components:

 - [Podping.cloud](https://github.com/Podcastindex-org/podping.cloud) - Podping.cloud is the hosted front-end to the 
                   Podping system.  It stands in front of the back-end writer(s) to provide a more friendly HTTP based 
                   API.
 - [Podping-hivewriter](https://github.com/Podcastindex-org/podping-hivewriter) - A backend writer that writes events 
                        to the Hive blockchain.
 - [Podping-hivewatcher](https://github.com/Podcastindex-org/podping-hivewatcher) - A script that anyone can run to 
                         watch the Hive blockchain for Podping events in real time. 
 - [Podping-hivewatcher-js](https://github.com/Podcastindex-org/podping-hivewatcher-js) - A Javascript version of the 
                            hive watcher script. 
 - [Podping-schemas](https://github.com/Podcastindex-org/podping-schemas) - The capnproto schema definitions used by 
                     Podping.cloud and the backend writers.
 - [Podping-schemas-python](https://github.com/Podcastindex-org/podping-schemas-python) - The Python version of the 
                            schema definitions.
 - [Podping-schemas-rust](https://github.com/Podcastindex-org/podping-schemas-rust) - The Rust version of the schema 
                          definitions.