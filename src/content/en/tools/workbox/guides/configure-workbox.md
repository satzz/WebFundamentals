project_path: /web/tools/_project.yaml
book_path: /web/tools/_book.yaml
description: Service Worker Libraries.

{# wf_published_on: 2015-01-01 #}
{# wf_updated_on: 2017-05-16 #}

# Configure Workbox {: .page-title }

Out of the box Workbox comes set up with some default values for cache names
and log levels. This guide will cover how you can change these values and what
will happen as a result.

## Configure Cache Names

As you start to use Workbox, you'll notice that caches are created for you.

> TODO Screenshot of caches

By default, Workbox will only create two caches, one for precaching and one
for runtime caches. You can find out the name of these caches by looking at

> TODO Add code snippet for cache names

These cache names are made of three pieces of information:

```
<prefix>-<precache cache-name or runtime cache-name>-<suffix>
```

You can set individual pieces of the cache names or everything like so:

> TODO Add code example for changing cache names

**We strongly recommend** that you change the prefix for each of your projects
as this allows you to develop on multiple projects on `localhost` with the
same port number, without mixing up the caches (which can get confusing when
switching between projects).

But you can obviously customize the entire cache name if you'd like.

### Cache Names in Strategies

As an aside, above we discussed how you can change the default cache names
Workbox uses. If you assign a `cacheName` value to any other part of the
Workbox API's, a cache using that exact name will be used. **No prefix or
suffix** will be added to your `cacheName`.

> TODO Example calling out that cacheName will be exactly as passed in.

## Configure Log Levels

Workbox has two builds that you can use.

- A `developer` build which comes un-minified, has additional logging
and assertion checks to make development easier.
- A `production` build which is minified and is stripped of any unneccessary
logging and assertion checks.

By default, when you use the developer build, all logs from Workbox will be
printed to the console, where as production will only print warnings and errors
to the console.

> TODO Screenshot of dev vs prod

You can determine the level of a log from the color code:

> TODO Link color to severity

But you can change the level of logging in both of these like so:

> TODO Include code snippet of altering log levels

This will reduce the amount of logging you'll see.
