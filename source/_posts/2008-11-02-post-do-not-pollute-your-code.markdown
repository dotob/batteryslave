---
layout: post
title: "Do not pollute your code"
date: 2008-11-02 14:31
comments: true
---

Last week a had another episode where i realized how important it is to remove code and files u are not shure why they are there. ok i have to admit i am still living in an environment with about 500k lines of code and a coverage far away from 10%. so testing was not our strength. so it is a bit harder to know if the code is used anywhere. 
  
but still. to pollute your source-tree with stuff you dont need anymore but are afraid to loose is a bad habit. of course you should have source control to have the security to return to a point in the past where the code existed. but dont comment dozens of lines in production-code. it makes it hard to read and probably the next time you are going to implement the commented code your u are smarter and will do it better.
  
as i like to say: removing old code is as nice as producing new one.
  
happy removing...
 