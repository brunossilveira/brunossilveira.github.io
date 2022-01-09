---
layout: blog
title:  "Fixing jekyll serve on Ruby 3"
date:   2022-01-09
categories: ruby
---

# Fixing jekyll serve command on Ruby 3

After a _long_ time without messing with this website I found myself not being able to run jekyll locally:

`bundle exec jekyll serve`

```
jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `<top (required)>'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:179:in `require_relative'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:179:in `setup'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:100:in `process'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `block in process_with_graceful_fail'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `each'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `process_with_graceful_fail'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:86:in `block (2 levels) in init_with_program'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `block in execute'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `each'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `execute'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary/program.rb:44:in `go'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary.rb:21:in `program'
	from /home/brunosilveira/.gem/ruby/3.0.0/gems/jekyll-4.2.0/exe/jekyll:15:in `<top (required)>'
	from /home/brunosilveira/.gem/ruby/3.0.0/bin/jekyll:23:in `load'
	from /home/brunosilveira/.gem/ruby/3.0.0/bin/jekyll:23:in `<top (required)>'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli/exec.rb:63:in `load'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli/exec.rb:63:in `kernel_load'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli/exec.rb:28:in `run'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli.rb:497:in `exec'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/vendor/thor/lib/thor/command.rb:27:in `run'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/vendor/thor/lib/thor/invocation.rb:127:in `invoke_command'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/vendor/thor/lib/thor.rb:392:in `dispatch'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli.rb:30:in `dispatch'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/vendor/thor/lib/thor/base.rb:485:in `start'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli.rb:24:in `start'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/gems/3.0.0/gems/bundler-2.2.3/libexec/bundle:49:in `block in <top (required)>'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/friendly_errors.rb:130:in `with_friendly_errors'
	from /home/brunosilveira/.rubies/ruby-3.0.0/lib/ruby/gems/3.0.0/gems/bundler-2.2.3/libexec/bundle:37:in `<top (required)>'
	from /home/brunosilveira/.rubies/ruby-3.0.0/bin/bundle:23:in `load'
	from /home/brunosilveira/.rubies/ruby-3.0.0/bin/bundle:23:in `<main>'
```

The fix?

`bundle add webrick`

You're welcome!
