---
layout: post
title: "getting ruby on rails to run on my debian 4 vps"
date: 2009-10-13 14:29
comments: true
---

man...always fighting to get a recent version of anything to run on the debian 4 (etch) vps i have. this time: ruby on rails. ruby v1.9.1, gems 1.3.5 and ror 2.3.4 is what i had i mind. 

first step: download ruby. execute the usual ./configure && make && make install triplet. worked fine. then download ruby gems. but when i tried to run anything with gem i got:

``` 
/usr/local/lib/ruby/site_ruby/1.8/rubygems/custom_require.rb:31:in `gem_original_require': no such file to load -- zlib (LoadError)
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/custom_require.rb:31:in `require'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/spec_fetcher.rb:1
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/custom_require.rb:31:in `gem_original_require'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/custom_require.rb:31:in `require'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/commands/update_command.rb:5
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/custom_require.rb:31:in `gem_original_require'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/custom_require.rb:31:in `require'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/command_manager.rb:167:in `load_and_instantiate'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/command_manager.rb:88:in `[]'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/command_manager.rb:144:in `find_command'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/command_manager.rb:131:in `process_args'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/command_manager.rb:102:in `run'
	from /usr/local/lib/ruby/site_ruby/1.8/rubygems/gem_runner.rb:58:in `run'
	from /usr/local/bin/gem:21
```

after googleing and trying some stuff i solved it this way:
- edit ruby-1.9.1-p0/ext/Setup
- uncomment the line zlib
- make && make install again

next step. successfully let gem install rails. but then ``ruby script/server`` stopped with:

```
=> Booting WEBrick=> Rails 2.3.4 application starting on http://0.0.0.0:3000/usr/local/lib/ruby/gems/1.9.1/gems/rails-2.3.4/lib/initializer.rb:271:in `rescue in require_frameworks': no such file to load -- openssl (RuntimeError)
	from /usr/local/lib/ruby/gems/1.9.1/gems/rails-2.3.4/lib/initializer.rb:268:in `require_frameworks'
	from /usr/local/lib/ruby/gems/1.9.1/gems/rails-2.3.4/lib/initializer.rb:134:in `process'
	from /usr/local/lib/ruby/gems/1.9.1/gems/rails-2.3.4/lib/initializer.rb:113:in `run'
	from /var/www/rails/ukk/config/environment.rb:9:in `<top (required)>'
	from /usr/local/lib/ruby/gems/1.9.1/gems/activesupport-2.3.4/lib/active_support/dependencies.rb:156:in `require'
	from /usr/local/lib/ruby/gems/1.9.1/gems/activesupport-2.3.4/lib/active_support/dependencies.rb:156:in `block in require'
	from /usr/local/lib/ruby/gems/1.9.1/gems/activesupport-2.3.4/lib/active_support/dependencies.rb:521:in `new_constants_in'
	from /usr/local/lib/ruby/gems/1.9.1/gems/activesupport-2.3.4/lib/active_support/dependencies.rb:156:in `require'
	from /usr/local/lib/ruby/gems/1.9.1/gems/rails-2.3.4/lib/commands/server.rb:84:in `<top (required)>'
	from script/server:3:in `require'
	from script/server:3:in 
```

after a another round i found [this site](http://linuxnuggetz.blogspot.com/2009/06/ruby-on-rail-on-ubuntu.html) with help:
- apt-get install openssl libssl-dev
- apt-get install ruby1.9-dev
- cd ruby-1.9.1-p129/ext/openssl
- ruby extconf.rb
- make && make install

that worked. nice. so now i am waiting for the next fail
 