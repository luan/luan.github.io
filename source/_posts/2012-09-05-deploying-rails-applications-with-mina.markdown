---
layout: post
title: "Deploying Rails Applications With Mina"
date: 2012-09-05 18:56
comments: true
categories: 
  - Deploy
  - Rails
  - Mina
  - Server
  - Ubuntu
---

{% raw %}

I recently participated on [StartupDEV Rumble](http://startupdev.com.br/rumble/pt/)
with my friend [Raphael Costa](http://raphaelcosta.net). Where we had to develop
and deploy a web application in 48 hours.

Besides being an awesome experience I learned a new deployment tool which is
really awesome: Mina.

<!-- more -->

[Mina](http://nadarei.co/mina/) is an awesome tool that deploys your application
blazingly fast, over a single ssh connection.

The first thing you have to do is set-up your VPS which should be easy enough,
there are plenty of guides around.

I just wanna share my Mina configuration for now.

First, add mina to your gemfile:

```ruby
gem 'mina'
```

Then create a file on `config/` folder call it `deploy.rb` with these contents
(watch for CAPS for where you have to change).

```ruby
require 'mina/bundler'
require 'mina/rails'
require 'mina/git'

# Basic settings:
# domain     - The hostname to SSH to
# deploy_to  - Path to deploy into
# repository - Git repo to clone from (needed by mina/git)
# user       - Username in the  server to SSH to (optional)

set :shared_paths, [
  'config/database.yml',
  'config/settings/production.yml',
  'log/production.log',
  'log/unicorn.log',
  'public/system',
  'public/uploads'
]

set :domain, 'YOUR_APP_DOMAIN'
set :deploy_to, 'YOUR_APP_FOLDER'
set :repository, 'YOUR_GIT_REPO'
set :user, 'deployer'

desc "Deploys the current version to the server."
task :deploy do
  deploy do
    # Put things that will set up an empty directory into a fully set-up
    # instance of your project.
    invoke :'git:clone'
    invoke :'deploy:link_shared_paths'
    invoke :'bundle:install'
    invoke :'rails:db_migrate'
    invoke :'rails:assets_precompile'

    to :launch do
      # WATCH HERE FOR YOUR UNICORN/SERVER service
      queue 'touch tmp/restart.txt'
      queue 'service unicorn restart'
    end
  end
end

desc "Shows logs."
task :logs do
  queue %[cd #{deploy_to!} && tail -f shared/log/production.log]
end
```

And really, that's all there's to it, I might post a full deploy guide soon if
someone ask for it.

{% endraw %}
