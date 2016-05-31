---
layout: post
title: "#RAD with Sinatra Part 2"
date: 2016-05-31 22:02:15 +0200
comments: true
categories:
---
[Previously](/blog/2016/05/29/number-rad-with-sinatra) on my blog I was covering Rapid app development with sinatra and Hazel,
So far I've created basic app skeleton, add some index and view action to our blog like hello world.

In this post we will try to complete other REST actions like Creating Updatind Deleting a blog post.

<!-- more -->

I'd like to notice one thing on the begining. Hazel is not that RAD As i wanted it to be, comparing to Rails when you simply type

```
rails generate scaffold Post title:string body:text
```

and that's all. In sinatra we have to do a bit more work.

##Missing actions

There is only two actions posts an view post, so it need to be extended:

```ruby
class HelloBlog < Sinatra::Base

  ## IMPORTANT set this to true so you can set request method in form using hidden field
  ## modern browser don't support put delete patch requests :(
  set :method_override, true

  set :public_folder => "public", :static => true

  get "/" do
    erb :posts
  end

  get "/posts/:id" do
    @post = Post.find(id: params[:id])
    erb :post
  end

  get "/post/new" do
    erb :form_post
  end

  post "/posts" do
    Post.new(params[:post]).save
    redirect "/"
  end

  get '/post/:id/edit' do
    @post = Post.find(id: params[:id])
    erb :form_post
  end

  put '/posts/:id' do
    Post.find(id: params[:id]).set(params[:post]).save
    redirect "/"
  end

  delete '/posts/:id' do
    Post.find(id: params[:id]).delete
    redirect "/"
  end

end
```

##Form Post view

I'm bit lazy, so I create only one view for create and update :)

views/form_post.erb
```html
<h2><%= @post.nil? ? "Create" : "Update" %> Post</h2>

<div class="content">
  <form action="<%= url @post.nil? ? "/posts" : "/posts/#{@post.id}" %>" method="post">
    <% unless @post.nil? %>
      <!-- NOTICE HOW METHOD PUT IS OVERRIDEN -->
      <input name="_method" type="hidden" value="put" />
    <% end %>
    <label>Title</label>
    <input type="text" name="post[title]" value="<%= @post ? @post.title  : "" %>"/>
    <label>Body</label>
    <input type="text" name="post[body]" value="<%= @post ? @post.body : "" %>"/>
    <input type="submit"  value="<%= @post.nil? ? "Create" : "Update" %>"/>
  </form>
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

##Add buttons to previous views:

views/posts.erb
```html
<h2>Posts</h2>

<div class="content">
  <ul>
    <% Post.all.each do |e| %>
      <li>
        <a href="<%= url "posts/#{e.id}" %>">
          <h4><%= e.title %></h4></br></a>
          <p><%= e.body.truncate(10) || "N/A" rescue "N/A" %></p>
      </li>
    <% end %>
  </ul>

  <!-- ADD THIS LINE -->
  <a href="<%= url "/post/new" %>">Create new</a>

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

views/post.erb

```html
<h2><%= @post.title %></h2>

<div class="content">
  <p>
    <%= @post.body %>
  </p>

  <!-- add this section with link and delete button -->
  <a href="<%= url "/post/#{@post.id}/edit" %>">Edit</a>
  <form action="<%= url "/posts/#{@post.id}" %>" method="post">
    <input name="_method" type="hidden" value="delete" />
    <input type="submit" value="Delete" />
  </form>
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

And thats it, simple hello world scaffold in sinatra. In rails you get that by executing one command, so thats not end of my journey, I'll keep looking for better solutions :) see you again on my blog.

all code is avalible on [github repo](https://github.com/marek2901/sinatra-scaffold-blog)
