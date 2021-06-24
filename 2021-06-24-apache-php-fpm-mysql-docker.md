---
title: "Apache + PHP FPM + Mysql with docker"
description: "It was about time, I decided to update my stack and use docker on my servers. Let's see how I built my LAMP stack with Docker (should we say DAMP?)"
image: /images/writing/ia-docker-php.jpg
categories: blog
comments: https://github.com/jrmgx/public/issues/2
tags: [dev, web]
---

# Introduction

It was about time, I decided to update my stack and use docker on my servers.

For the ones that follows me I usually prefer to set up a bare metal server with Debian Apache PHP and MySQL installed on it.  
My excuses — if I even need some — were a mix between performances and simplicity.

But today I'm experimenting with multiple small projects, and they all are on the same server.
It's still a bare metal server, and I'm not — yet — changing this for a cloud provider, we can talk about this in another blog post.

Multiple projects with a single server can lead to some incompatible dependencies,
in my specific scenario I need some PHP extension that is obsolete and works only with older PHP version, but I don't want to carry this limitation to all my projects.

Why Apache and not Nginx? [No reason &copy;](https://youtu.be/-rmf5EqJVDw?t=16).

Let's see how I built my LAMP stack with Docker (should we say DAMP?)

# PHP

I used the `php:7.4-fpm` base image and added some extensions, those vary depending on the current project.

I added composer because it's always handy to have it there, plus, and this may be up for discussion, I added NodeJS + PM2 in this image too.

Node is often needed for the front end (compiling asset and such) and I did not want the complexity of another image (but I'm open for comment/PR on this one)
as well as [PM2](pm2.keymetrics.io/) that I use for my background scripts. 
For example like this: `pm2 start --name php_messenger_consume php -- bin/console -n messenger:consume async --limit=10 --memory-limit=128M --time-limit=3600`

It's very handy to have it on the same image as PHP, and does not look even possible otherwise (let me know in the comments).

<code class="filepath">file:///docker/php/Dockerfile</code>
<pre><code class="language-dockerfile">
{{ include('docker/php/Dockerfile')|e('html') }}
</code></pre>

# Apache

This on is a bit more tricky.

First on my production server I won't use it, the server will have a real Apache with vhosts configured properly forwarding to each different projects.
Those final vhosts will look similar to the one below that I use in the development image.

So for the dev env (hence the `docker-compose-dev.yml` coming up below) it will have Apache.

The goal is to redirect the query that hit the server to PHP FPM.

For this, I first tried to use the base image `httpd:2.4` it seemed logic to me but for some reason I never succeeded to make it work, (have you? PR welcome).
I ended up using a debian base `debian:buster-slim` and installed Apache on my own (probably not optimal, but we are on a dev environment).

It has *mod proxy fast CGI* and *mod rewrite* activated as we will use it in our config.

<code class="filepath">file:///docker/apache/Dockerfile</code>
<pre><code class="language-dockerfile">
{{ include('docker/apache/Dockerfile')|e('html') }}
</code></pre>

# MySQL

I'm using the base image `mysql` with some options like `MYSQL_ALLOW_EMPTY_PASSWORD` and `cap_add: SYS_NICE`. 
The later prevent the error message "mbind: Operation not permitted" as seen on [stackoverflow.com](https://stackoverflow.com/a/55706057/696517)

# Connecting everything together

The idea now it to put everything together: 

 - Apache will be exposed to some port and redirect requests to PHP-FPM.
 - PHP will be able to connect to the database. 
 - And also we can use PHP as a command line on our server to execute PHP commands.

I added comments on the file directly, so you have the context.

<small>Note: the name of the project is "bookmark" in this example</small>

## Production

<code class="filepath">file:///docker-compose.yml</code>
<pre><code class="language-yaml">
{{ include('docker-compose.yml')|e('html') }}
</code></pre>

This is the version used in production, so without Apache.

Now the version for local dev

## Development

As you probably now, docker-compose config files cascade (like CSS), and we can supersede options
Again I added comment strait in the file for context.

<code class="filepath">file:///docker-compose-dev.yml</code>
<pre><code class="language-yaml">
{{ include('docker-compose-dev.yml')|e('html') }}
</code></pre>

## Glue in between

Be it in production or development, having apache on the server or in a docker image,
you know have to redirect web request to PHP FPM

This is done with an Apache config file (here called `vhosts.conf` for historical reasons)

This config file is similar between a production environment and a dev environment,
main differences will be explained in inline comments

Note: the base version here is the dev environment.

<code class="filepath">file:///apache/conf/vhosts.conf</code>
<pre><code class="language-apache">
{{ include('docker/apache/conf/vhosts.conf')|e('html') }}
</code></pre>

<link rel="stylesheet" href="{{ site.url }}/assets/css/monokai.min.css">
<script type="text/javascript" src="{{ site.url }}/assets/js/highlight.min.js"></script>
<script>hljs.highlightAll();</script>
