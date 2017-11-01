---
layout: post
title:  "One Side-Channel to Bring Them All and in the Darkness Bind Them: Associating Isolated Browsing Sessions"
image: ''
date:   2017-09-23 00:06:31
tags:
- Web-Privacy, Side-Channel, Tom_Van_Goethem, Wouter_Joosen
description: 'Review on the paper One Side-Channel to Bring Them All and in the Darkness Bind Them: Associating Isolated Browsing Sessions'
categories:
- Web, Web Privacy
---

After reading through papers intensively on Web Privacy, I was currently reading a recently released paper in USENIX, <a href="https://www.usenix.org/conference/woot17/workshop-program/presentation/van-goethem">One Side-Channel to Bring Them All and in the Darkness Bind Them: Associating Isolated Browsing Sessions</a>. In recent findings, researchers have shown that unwanted web tracking is on the rise, as advertisers are trying to capitalize on users’ online activity, using increasingly intrusive and sophisticated techniques. Among all the techniques that has been found, browser fingerprinting has received the most attention since it allows trackers to uniquely identify users despite the clearing of cookies and the use of a browser’s private model.

In the research paper <a href="http://ieeexplore.ieee.org/document/7958618/"> XHOUND: Quantifying the Fingerprintability of Browser Extensions</a>, the authors <a href="https://twitter.com/nicknikiforakis"> Nick Nikiforakis</a> and <a href ="https://twitter.com/o_starov"> Oleksii Starov </a>, have investigated and quantified the fingerprintability of browser extensions, such as, AdBlock and Ghostery which most of the users rely upon. The authors have shown that an extension’s organic activity in a page’s DOM can be used to infer its presence.

In the paper <a href ="http://randomwalker.info/publications/OpenWPM_1_million_site_tracking_measurement.pdf">Online Tracking: A 1-million-site Measurement and Analysis</a> by <a href="https://twitter.com/search?q=Steven%20Englehardt&src=typd">Steven Englehardt</a> and <a href="https://twitter.com/random_walker">Arvind Narayanan</a> released the measurement of the top 1 million sites using the self-developed tool which gave an answers to the question "Who are the largest trackers?", "Which sites embed the largest number of trackers?" and "Which tracking technologies are used, and who is using them?". The exposure of the data made many of the trackers to withdraw their tracking from several websites or change their way of tracking.

As the title of this article suggests, here it is all about the paper which was published recently in USENIX. In this paper, the authors <a href ="https://twitter.com/tomvangoethem">Tom Van Goethem</a> and Wouter Joosen evaluate the browser’s threat surface with regard to fingerprinting and tracking in the context of isolated browsing sessions which is between regular browsing sessions versus the incognito sessions. The authors also show how the IP address of Tor users are revealed when they use a concurrent browsing session.

The authors bring provide three major/key contributions which are little explored in the area of privacy which are
<ol>
<li>Shows that Side-Channel attacks on modern web browsers are a serious threat for fingerprinting</li>
<li>The authors discover three Side-Channel attacks that can be abused to track users along private browsing mode.</li>
<li>The proposed fingerprinting technique can be used to reveal the IP address of Tor users.</li>
</ol>
