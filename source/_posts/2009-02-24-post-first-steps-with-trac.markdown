---
layout: post
title: "first steps with trac"
date: 2009-02-24 21:36
comments: true
---
in a recent little project of my alter ego dotob (website not yet nice and tasty) i wanted to try the project management tool [trac](http://trac.edgewall.org/) from edgewall. 
its a webbased system written in python. nice because i want to look at python as a scripting language anyway.so because my webserver is a debian system the installations first steps where easy:

``apt-get install trac``

after that trac is installed in /usr/share/trac. to create your trac-environment you have to call the admin console program from trac:

``trac-admin /var/www/trac initenv``

where /var/www/trac is the place where your website will go and initenv is the command to tell trac-admin to initialise a new environment. next i setup apache to serve the new site. there are a bunch of possibilities to let apache server trac: cgi, fastcgi, mod_python etc. i tried cgi and fastcgi first but the mod_python version worked at last. so i need to install mod_phyton:

``apt-get install libapache-mod-python``

ok got some tips for the site config out of the net. here is my virtualhost-config part:

  # Trac Configuration
  <VirtualHost *>
    ServerName wop.dotob.de
    DocumentRoot /var/www/trac/
    <Location >
      SetHandler mod_python
      PythonHandler trac.web.modpython_frontend
      PythonInterpreter main_interpreter
      PythonOption TracEnv /var/www/trac/
      PythonOption TracUriRoot /
      AuthType BasicAuthName "trac"
      # Use the SVN password file.
      AuthUserFile /var/svn/.dav_svn.passwd
      Require valid-user
    </Location>;
  </VirtualHost>

sorry i assumed you have set your subversion on the same server an it is already up but i wont tell how this is done here. thats said: the /var/svn/.dav_svn.passwd is my subversion authentication file. i use the svn authentication for trac too.well now trac is up and running. you can use trac-admin, which is a great commandline tool with tab-completion and all to config stuff like when you create a ticket which level of severity are displayed or what the name of your milestones is. what i suggest also is to set some of the user permissions so you can change more stuff via the webinterface.

another thing i wanted is to let trac look into subversion commits an retrieve some information from my commit messages. so assume i have a ticket #5 and i check some fix for that. now i can write "fix #5 introduce new style in xaml" in the commit message and trac will close the ticket #5 with the svn version mentioned and the commit message attached. so how is this done:subversion uses hook files to let users run scripts. so you go to your subversion repository (assume /var/svn/src) into the directory hooks. rename the post-commit.tmpl to post-commit. than that file is executed by subversion. now you need to insert the code from trac:

``REPOS="$1"REV="$2"LOG=`svnlook log -r $REV $REPOS`AUTHOR=`svnlook author -r $REV $REPOS`TRAC_ENV='/var/www/woptrac'/usr/bin/python /usr/share/doc/trac/contrib/trac-post-commit-hook -p "$TRAC_ENV" -r "$REV" -m "$LOG" -u "$AUTHOR"``

i found some tutorials where the LOG part was missing but i couldnt get it to work without it. another thing that cost me a lot of time: when you write the commit it didnt work for me to write: "Fixes: #5". i think the : between the command Fiexs and the #5 was not considered by the scripts regex.i am curious about using trac. will tell how it behaves. 
