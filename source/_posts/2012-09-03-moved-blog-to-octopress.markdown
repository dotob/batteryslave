---
layout: post
title: "moved blog to octopress"
date: 2012-09-03 21:14
comments: true
categories: 
---

finally its done: my move to octopress is almost finalized. i previously used [wordpress](http://www.wordpress.org). for the first migrationstep i used a small ruby script inspired by [this](https://github.com/sbooch/wp-import)

``` ruby
require 'rss/2.0'
require 'rss/content'
require 'schnitzelpress'
require './downmark_it.rb'

# Enter the name of your exported Wordpress articles here (in WP, go to Tools - Export)
# must be in the same folder as this script!
source = "wordpress.xml"

content = ""

open(source) do |s| content = s.read end
rss = RSS::Parser.parse(content, false)

i = 0

while i < rss.items.size

slugs = [ rss.items[i].link[rss.items[i].link.rindex('/', rss.items[i].link.rindex('/')-1)..-1].chomp('/')[1..-1] ]
	title = rss.items[i].title
	fname = title.strip.downcase.gsub(/[\s\.\/\\]/, '-').gsub(/[^\w-]/, '').gsub(/[-_]{2,}/, '-').gsub(/^[-_]/, '').gsub(/[-_]$/, '')
	inhalt = DownmarkIt.to_markdown(rss.items[i].content_encoded)
	date = rss.items[i].pubDate.strftime("%Y-%m-%d %H:%M")
	shortdate = rss.items[i].pubDate.strftime("%Y-%m-%d")
	doc = "---\nlayout: post\ntitle: \"#{title}\"\ndate: #{date}\ncomments: true\n---\n#{inhalt} "
	File.open("#{shortdate}-post-#{fname}.markdown", 'w') {|f| f.write(doc) }
	i += 1
end
```

some posts are not totally beautifully markdowned but i am working on that.

so have fun