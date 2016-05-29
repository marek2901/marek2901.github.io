---
layout: post
title: "#RAD with Sinatra part 1"
date: 2016-05-29 16:25:49 +0200
comments: true
categories:
---

##RAD

RAD - Rapid Application Development, it's a style of creating applications, that allows you get things done quickly,
this philosophy is highly used in Rails framework.

<!-- more -->

##Sinatra

For those of you who might not know sinatra is minimalistic ruby web framework, to run simple hello world simply install gems

```
gem install sinatra
```
Create file named for example `sinatra_hello.rb` with this content

```ruby
require "sinatra"

get '/' do
  "Hello World"
end
```

now simple command

```
ruby sinatra_hello.rb
```

And it's done!! Simple web server created!!

But wait, sure we can write web server one liner. But what with database connection, templating system, getting users params.
All that you have to code by yourself, which means lot of work, and many programming decisions that can make things bad!

So Sinatra is not RAD framework at all!!

##The Challenge

Sinatra doesn't have rad features by default, but with a bit help of community we can achieve rails like development style.

There is tool called <a target="_blank" href="https://github.com/c7/hazel">hazel</a> that lets you help creating sinatra app quickly reallyy guickly!!

###Generate template

Let's use postgresql for database engine (you can have more options like mysql,redis)
To begin simply type

```
hazel hello_blog -d postgresql --bundle
```

run --bundle option for automatic bundle install

Output:
```
      create  hello_blog/config/initializers
      create  hello_blog/lib
      create  hello_blog/spec
      create  hello_blog/db/migrate
      create  hello_blog/lib/.gitkeep
      create  hello_blog/public/stylesheets
      create  hello_blog/public/stylesheets/main.css
      create  hello_blog/public/javascripts
      create  hello_blog/public/javascripts/.gitkeep
      create  hello_blog/public/images
      create  hello_blog/public/images/.gitkeep
      create  hello_blog/public/images/hazel_icon.png
      create  hello_blog/public/images/hazel_small.png
      create  hello_blog/public/favicon.ico
      create  hello_blog/views
      create  hello_blog/views/layout.erb
      create  hello_blog/views/welcome.erb
      create  hello_blog/hello_blog.rb
      create  hello_blog/spec/hello_blog_spec.rb
      create  hello_blog/spec/spec_helper.rb
      create  hello_blog/config.ru
      create  hello_blog/Gemfile
      create  hello_blog/Rakefile
      create  hello_blog/README.md
      create  hello_blog/config/db.yml
      create  hello_blog/config/initializers/database.rb
         run  bundle from "./hello_blog"
```

now setup your database connection:

```ruby
development:
 adapter: postgresql
 host: localhost
 database: blog
 user: user
 password: password
```

`warning, you need to have rake commands avalible, in my case there were issue with them, rake couldn't found environment task, so I've created one:`

Rakefile
```
task :environment do
  require "./env_loader"
  require "./hello_blog"
end
```
As you see i created env_loader in main dir, it basically extracts few first lines from config.ru so i can reuse them here

Content of env_loader extrated from config.ru
```ruby
$LOAD_PATH << File.expand_path(File.dirname(__FILE__))

#set default env as development can be overriden
ENV['RACK_ENV'] ||= "development"

require "bundler"
Bundler.require

# Local config
require "find"

%w{config/initializers lib}.each do |load_path|
  Find.find(load_path) { |f|
    require f unless f.match(/\/\..+$/) || File.directory?(f)
  }
end
```

modified config.ru

```ruby
# Load path and gems/bundler
require "./env_loader"

# Load app
require "hello_blog"
run HelloBlog
```

run
```
rake spec
```
to check if it's working.

now there is a base setup so lets create post CRUD

* Create model and migration

Inside lib folder create post.rb

```ruby
class Post < Sequel::Model

end
```
simple Post class extending Sequel Model


Next inside db/migrate folder create new file:

db/migrate/001_create_posts.rb

`remember sequel migration naming convention [nomber]_description.rb, you will get errors if you don't follow.`

```ruby
Sequel.migration do
  up do
    create_table(:posts) do
      primary_key :id
      column :title, :text, :unique=>true
      column :body, :text, :unique=>true
    end
  end

  down do
    drop_table(:posts)
  end
end
```

Now simply run

```
rake db:migrate
```
If everything is done correctly you shouldn't see any message, if you do:

-- you could forgot creteing database, simply login to your db shell and type
```sql
CREATE DATABASE blog;
```
-- add `"postgres"` to Gemfile and run bundle install

* Create views

-- Index

create `views/posts.erb`

```html
<h2>Posts</h2>

<div class="content">
  <ul>
    <% Post.all.each do |e| %>
      <li>
        <a href="<%= url "posts/#{e.id}" %>">
          <h4><%= e.title %></h4></br></a>
          <p><%= e.body.truncate(10) || "N/A" %></p>
      </li>
    <% end %>
  </ul>
</div>

<div class="sidebar">
  <h3>Environment</h3>
  <ul>
    <li><b>Ruby:</b> <%= RUBY_VERSION %></li>
    <li><b>Environment:</b> <%= ENV["RACK_ENV"] %></li>
    <li><b>Server:</b> <%= @env["SERVER_SOFTWARE"] %></li>
    <li><b>Port:</b> <%= @env["SERVER_PORT"] %></li>
  </ul>
</div>
```

next add mapping in app class:

```ruby
class HelloBlog < Sinatra::Base

  set :public_folder => "public", :static => true

  get "/" do
    erb :posts
  end
end
```

-- View single post

in index file there is href in title:
```html
<a href="<%= url "posts/#{e.id}" %>">
```
so we need to map /posts/:id in our app class

```ruby
class HelloBlog < Sinatra::Base

  set :public_folder => "public", :static => true

  get "/" do
    erb :posts
  end

  get "/posts/:id" do
    erb :post
  end
end
```

## TO BE COUNTINUED

thats the first part of simple sinatra scaffold in next part we will cover creating deleting and updating our post

all code is avalible on [github repo](https://github.com/marek2901/sinatra-scaffold-blog)
