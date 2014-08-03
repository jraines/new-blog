---
title: Heroku Cost Optimization for Rails Apps
date: 2014-05-10
tags: heroku
---


# Heroku Cost Optimization for Rails Apps

If you're thinking about choosing (or have already chosen) Heroku for your Rails
application, you probably already know that their well-designed platform,
developer tools, documentation, and rich add-on ecosystem will save you lots of
money in devops time.  The tradeoff, of course, is more of your money going to
Heroku as you add more dynos as your app grows, and pay for more third-pary
addons.  This article will offer a few tips for keeping those costs down while
maintaining app performance and agility.

## Use a web server that can process requests concurrently

An out-of-the-box Rails app on Heroku will process one request at a time.
Because Heroku uses [random request
routing](https://devcenter.heroku.com/articles/http-routing#request-distribution),
if a request goes to a dyno which is busy, it will be queued, and the user will
experience decreased responsiveness from your app.

With a concurrent web server like Unicorn, your dynos can process multiple
requests simultaneously. With a little experimentation (perhaps with load
testing from [blitz.io](https://www.blitz.io) and monitoring via [New
Relic](http:/www.newrelic.com) -- both available as Heroku add-ons), you'll
determine the right number of unicorn processes to run per dyno for your app's
needs.

An increasingly popular server choice is
[Puma](https://devcenter.heroku.com/articles/deploying-rails-applications-with-the-puma-web-server),
which also allows concurrent requests, but can also scale out via threads in
addition to worker process. Your app must be thread-safe to take advantage of
that capability.

## Experiment with different dyno size options

A standard Heroku dyno comes with 512mb of RAM. When using a multi-process or
multi-threaded server solution, you'll be loading multiple copies of your Rails
environment into memory.  However, there's a baseline footprint and some
possible optimzations (which we'll get to), which means that you may get more
value by running more worker processes on a smaller number of higher-memory
dynos, versus a smaller number of workers spread across many standard dynos.

A new option from Heroku is their [XL
dyno](https://blog.heroku.com/archives/2014/2/3/heroku-xl), which is equivalent
to 12x a standard dyno. The potential "faster and more consistent response
times" is tempting if you're in this usage range, but offers less elasticity in
your scaling (and, therefore, your montly bill).

## Use a dyno manager to scale down in off-peak times

With New Relic and other performance monitoring tools, it's not particularly
hard to write a script that will monitor your app's usage and scale your dyno
numbers up or down accordingly. However, because of its low price and
configuration options, we use [hirefire.io](http://www.hirefire.io) for
this, and it has saved us a considerable amount of money.  We set more aggresive
downscaling thresholds for our non-production environments for additional
savings. Update: I recommend using a fairly tolerant threshold for scaling up if
you connect this to New Relic and base it on Apdex score.

## Reduce number of background workers with Sidekiq or Queue Classic

As most apps grow, they build up quite a large repertoire of tasks that need to
be performed outside of the web request/response cycle.  Many Rails apps use
Resque for this, and eventually need to run multiple worker processes to consume
their background job queues.  It's easy to forget about these workers chugging
away in the background, pumping up your monthly bill.

There are a few options to save money here.  With Ryan Smith's
[queue_classic](https://github.com/ryandotsmith/queue_classic), a fast
database-backed queue and worker system, you could eliminate the potentially
expensive Redis dependency altogether.

For an easy transition and greater throughput, try Mike Pernham's
[Sidekiq](http://sidekiq.org), which takes advantage of a multithreaded,
actor-based architecture for increased performance.  According to Mike, he "took
one large Resque farm from 68GB of RAM to 1GB of RAM by using threads instead of
processes."

## Reduce number of expensive add-ons

This point will likely change due to competition within the Heroku add-on
ecosystem, but we've gotten a few wins by leveraging our existing, required
Heroku infrastructure to fill certain needs in lieu of a dedicated add-on.

1. Generally prefer memcached to Redis
2. Prefer the database to Redis for anything that isn't essentially caching
3. If search is a feature of your app, but not central to it, leverage Postgres
   or Mongo's native full-text search capabilities instead of a hosted search
service

## Reduce the number of requests to your app for static assets

At minimum, you should be [using Rack::Cache and
memcached](https://devcenter.heroku.com/articles/rack-cache-memcached-rails31)
to cache requests for static assets such as files, images, and scripts.

The next step is to not serve your assets from your app servers at all, but via
Amazon S3.  We use the [asset_sync
gem](https://github.com/rumblelabs/asset_sync) for this, which has been
excellent except for the increased time taken by each deploy (about 5 minutes,
for us).

The final step is to also place a CDN, such as Cloudfront, in front of those S3
assets.  Heroku has [a guide for
this](https://devcenter.heroku.com/articles/using-amazon-cloudfront-cdn), but I
would add a bit of warning:

If want to use SSL and have the assets served from domain (for example,
assets.smashingmagazine.com), the cost of setting that up with Cloudfront is
$600 per month.  And if you don't do it, be prepared to hack around things like
strict browser rules for cross-origin policy on webfonts, or putting CORS rules
in place for certain assets and making sure that those CORS headers are cached
with the Cloudfront response.

## Avoid tying up dynos with file uploads

Even if you're using a concurrent web server, your users' requests can still get
stuck behind others if some of your requests take a lot longer than others.  The
most common case for this is file uploads. Ideally, you can use a solution like
[CarrierWave Direct](https://github.com/dwilkie/carrierwave_direct) or
[Transloadit](https://www.transloadit.com) to route these around your Heroku
dynos and perform any post-processing outside of the request as well.

Because we need to accept uploads from mobile users, we built a separate node.js
server for handling uploads, which talks directly to S3 and then notifies our
Heroku app when it's time to process the upload, in the background.

Hopefully, these tips will save you some time, money, and headaches as your grow
your app on the Heroku platform.

