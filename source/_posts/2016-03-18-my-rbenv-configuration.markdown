---
layout: post
title: "Ruby Environment setup with rbenv"
date: 2016-03-18 17:14:02 +0100
comments: true
categories:
---

## Ruby On Your computer

There are few ways to get up and running with Ruby on your machine,
on OS X you have one right out of the box, on linux you can install it via
package managers (rpm, apt). But this approach gives you single out dated
ruby version you have to use. To avoid i use ruby env manager, so i can install
latest rubies (even developer previews), and switch them at runtime.

<!-- more -->

### 1. Why I Use rbenv

There are few ruby environment managers (rvm, rbenv, chruby), but i mostly use rbenv
because it's simple and has a lot of plugins. It allows to use different ruby versions
for different shell instances.

### 2. Installation

#### Linux/Ubuntu

1.Install dependecies

```
sudo apt-get update && sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
```

2.Install rbenv

```
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

rbenv install 2.2.3
rbenv global 2.2.3
ruby -v
```
