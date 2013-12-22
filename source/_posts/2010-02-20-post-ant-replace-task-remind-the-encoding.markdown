---
layout: post
title: "ant replace task: remind the encoding"
date: 2010-02-20 19:39
comments: true
---
puh man again nearly going crazy over a simple problem. 

the environment: 

- java project
- edited in eclipse (an a mac)
- check into subversion (on a linux box)
- teamcity buildserver using ant build script (same linux box as subversion)

what happened: everywhere the encoding was correct beside one file. in that file my umlauts where messed. in my eclipse build (warning: not using the ant there) everything was fine. but when is looked at the file in teamcitys build directory it was messed. at first i thought of subversion. checked everything  out by hand on the linux box => fine. then i tried to find out how teamcity uses svn. they have their own implementation. so i figured they do something wrong. so i updated teamcity from 4.5 to 5.0.2...
same result. so i waited about a week. then with a fresh mind i looked at the ant script when it hit me: i had a replace task to set the buildnumber in the messed sourcecode file (perhaps an unusual attemped but...). and replace uses the default encoding of the executing jvm. i neede to set the encoding for the javac task but i forgot or better didnt consider it a problem for the replace.with the encoding set everything was fine. see how:

``` xml
	<replace 
		file="MainWindow.java" 
		token="buildnumber" 
		value="${myBuildNumber}" 
		encoding="ISO-8859-1" 
	/>
```  