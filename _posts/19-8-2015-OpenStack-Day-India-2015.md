---
layout: post
title:  "OpenStack Day India 2015"
date:   2015-08-19
tags:
  - openstack
  - outreachy
  - opw
  - event
---

This post is dedicated to talk about an event [OpenStack Day India 2015][4], also I gave a talk there, so post also contain experience of giving first talk for OpenStack and Outreachy in public.

It was an amazing experience to be a part of OpenStack Day India 2015. It remind me [last OpenStack summit][3](17-22 May 2015). I got to learn things about OpenStack which I was not aware of and I was the only girl who was presenting a talk there, so was nervous but more excited because I was suppose to talk about my experience of '[Outreachy with OpenStack Zaqar][1]'. Also I got to meet many people there, few were the same whom I met in summit but many were new faces which made me happy as I meet with many girls to whom I have encouraged to participate in Outreachy and also I realized that so many people from India are involved with OpenStack.

It was a two days event. Day one devoted to hands-on sessions, and day two was full of talks by different people([agenda][2]), two panels were running in parallel so I had to made tough choices which talk to attend and which to leave. Let me talk about each day.

###Day One
Day One was full of practical knowledge, hands-on sessions. There was a team of organizers who have presented topics one by one. First they gave an overview of OpenStack and its main components, for totally naive people to get comfortable. Then they taught 'multi-node deployment' of OpenStack with very basic three nodes architecture(Controller, Network and Compute).

During this I got to know how exactly each component talk to other and how a part of a component talk to other parts of same component. The essence is when two components talk to each other they make API calls and when one part of component talk to other part of same component they use some messaging/queuing services to send and receive messages. Obviously there is no hard and fast rule, in most of the cases this happens. Since OpenStack installation need many things need to get install, the presenters were explaining about each and every software/tool that we were installing, why they needed and what are their functionalities. This part put light on loads of new things for me.

Then they talk about 'Getting Started with OpenStack development'. I was aware of most of the things presented in this session but I was enjoying by cross-checking everything I know and what the presenter is speaking.
 
###Day Two
This day was full of excitement because my talk was scheduled on this day. The conference hall was full of around 300-400 people. Very first I was delighted to listen the keynote from COO of OpenStack, [Mark Collier][5] about how people are using OpenStack. Then a small session for networking started, there I meet to many people who are contributing to OpenStack and was talking about the work they do. Then two panels started, each having separate talks, so as I said, I had to make tough choices, which to attend and which to leave. I liked some talks a lot like 'How to Write an app in OpenStack' by [Tom Fifield][6] and 'Database experiences - designing Cassandra backend for Keystone' by [Rushi Agarwal][7] & [Ajaya Agarwal][8]. Other than this not to mention about the yummy food all attendees got.

###My Presentation - Outreachy with OpenStack Zaqar
My talk was to share experience of 'Outreachy with OpenStack Zaqar'([slides][1]). I was the only girl who was presenting a talk there, other than me there was one more girl, [Vinothini Raju][9] who was on stage, in panel discussion for 'Impact of OpenStack in Emerging Market'.

Basically in the talk I explained about my journey with [OpenStack][11] through [Outreachy][10] from start to end. Why I wanted to contribute to open source projects and specifically [OpenStack][11], how I got to know about [Outreachy][10] and how I started contributing, about my initial contribution, few Road-Blocks, ups and downs, all things that I believe I have achieved while and after the internship.

Also I presented few things about my project during Outreachy internship- '[Split data/control planes of Zaqar][12]', little overview of [Zaqar][13] and its [benefits][14] because I have seen few people over there who were curious to know more about [Zaqar][12]. After the talk many girls approached me to know more about [Outreachy][10]. I was happy to discuss with all of them more and helping them to reach out to [Outreachy][10]/[OpenStack][11] and sharing about my experience. 

I hope more girls come ahead in tech and open source world, and I will be happy to help! 


[1]: http://exploreshaifali.github.io/talks/6-8-2015-outreachy-with-OpenStack-zaqar.html#/cover-page
[2]: https://docs.google.com/spreadsheets/d/1PTDUhhP-ntDyJSEqEyMOCGsD0sJ0sg2ofXgauiyvWJ8/edit#gid=0
[3]: https://www.OpenStack.org/summit/vancouver-2015
[4]: http://www.meetup.com/Indian-OpenStack-User-Group/events/223574830/
[5]: https://www.openstack.org/community/members/profile/31
[6]: https://twitter.com/tomfifield
[7]: http://www.rushiagr.com/
[8]: http://ajayaa.github.io/
[9]: https://twitter.com/vinothiniraju
[10]: https://wiki.gnome.org/Outreachy
[11]: http://openstack.org/
[12]: https://blueprints.launchpad.net/zaqar/+spec/split-data-and-control-plane
[13]: https://wiki.openstack.org/wiki/Zaqar
[14]: http://exploreshaifali.github.io/2015/07/28/what-why-zaqar/