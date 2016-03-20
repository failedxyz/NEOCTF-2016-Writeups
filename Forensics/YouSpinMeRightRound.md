---
layout: post
title: NeoCTF - You Spin Me Right Round Baby Write-Up (500)
date: 3-18-2016
author: Nick DeFilippis
---
Problem
===
Josh McCown is lost. Can you help him find his way again? https://dl.dropboxusercontent.com/u/12992225/helicopter.gif

tl;dr
===
Repeated ROT13-Base64

Write-Up
===

For this problem, we are given a gif of a football player. Checking the EXIF
using a tool such  as [this](http://regex.info/exif.cgi) data reveals a base64 string in a comment.

Trying to decode this string directly results in things that are not letters.
However, if you apply a ROT13 transformation, and then decode, you get another base-64 string.
Continuing to apply ROT13-base64 transformations results in a message:

**almost home! uryyb? vf gurer nalbar va urer? nu, lrf. gnxr guvf. synt{py3irynaq_oe0jaf<fcnp3_oe0jaf}**

Finally, we apply another ROT13 transoformation to get the flag:<br>
**nyzbfg ubzr! hello? is there anyone in here? ah, yes. take this. flag{cl3veland_br0wns<spac3_br0wns}**

Flag: flag{cl3veland_br0wns<spac3_br0wns}

*Write-Up by Nick DeFilippis*
