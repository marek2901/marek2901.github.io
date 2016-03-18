---
layout: post
title: "Ruby Environment setup with rbenv"
date: 2016-03-18 17:14:02 +0100
comments: true
categories: rbenv ruby
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

>Note: This is my personal setup with all my favorite plugins

#### Linux/Ubuntu

1.Install dependecies

```
sudo apt-get update && sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
```

2.Install rbenv
>Use .zshrc or bash_profile insted of bashrc depending on your shell

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

rbenv install 2.3.0
rbenv global 2.3.0

ruby -v
# should give output with proper ruby version
```

#### OS X

```
brew install rbenv ruby-build

echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.bash_profile
source ~/.bash_profile

rbenv install 2.3.0
rbenv global 2.3.0

ruby -v
# should give output with proper ruby version
```
## 3.Install plugins

* rbenv-rehash

Automaticaly executes rbenv rehash when new shims added

```
git clone https://github.com/sstephenson/rbenv-gem-rehash.git $(rbenv root)/plugins/rbenv-default-gems
```
* rbenv-default-gems

Install default gems at each ruby version

```
git clone https://github.com/rbenv/rbenv-default-gems.git $(rbenv root)/plugins/rbenv-default-gems

## always install bundler in new rubies
echo bundler >> $(rbenv root)/default-gems
```

* rbenv-bundle-exec

When working on more than one project with bundler dependency, sometimes
there is dependency mismatch, for example in one project you use rails 3
and another you use rails 4, without this plugin, we would have to
manually write `bundle exec <command>`

```
## no more need to prefix `bundle exec` in rails command

git clone https://github.com/maljub01/rbenv-bundle-exec.git $(rbenv root)/plugins/rbenv-bundle-exec
```
* rbenv-sudo

simply type `rbenv sudo <command>` to execute it in sudo session

```
git clone git://github.com/dcarley/rbenv-sudo.git $(rbenv root)/plugins/rbenv-sudo
```
