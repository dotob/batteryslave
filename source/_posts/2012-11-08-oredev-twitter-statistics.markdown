---
layout: post
title: "oredev twitter statistics"
date: 2012-11-08 23:18
comments: true
categories: code twitter oredev
---

with beeing at oredev one has to use twitter. so i did and it is was fun. got some new followers and found some interesting side infos and also some new guys to follow. but i wondered if there is some statistic data to gather around twitter usage. heres what i did:

- install t gem
```
gem install t
```
- authorize, follow the orders of t
```
t authorize
```
- query twitter and but just use stuff from beginning of conference on monday
```
t search all -n 10000 --csv "#oredev" | grep "2012-11-0[5-9]" | tee all_tweets
```
- get top 10 tweeter
```
awk -F, '{print $3}' all_tweets | sort | uniq -c | sort -n -r| head -n10
```
- time distribution of tweets grouped by hour
```
awk -F, '{print $2}' all_tweets | awk -F: '{print $1}' | uniq -c > time_distribution
```

## results

#### how many tweets in 5 days

2639

#### top 10

1.  141 lisacrispin
1.  115 eventmalin
1.   92 oredev
1.   79 jakobklamra
1.   73 rafek
1.   41 Aptitud_Sthlm
1.   40 stratiteq
1.   39 testertested
1.   38 steenhulthin
1.   36 siggeb

#### time distribution graph

![time distribution](/images/oredev_tweets_timedistribution.png)
unfortunately without meaningful x-axis lables...any ideas how to get that right are welcome.
