# Precaching in Workbox

Precaching in a service worker results in fantastic wins for performance and
is a requirement if you want your web app to work offline.

Workbox takes a lot of the heavy lifting out of precaching by simplifying the
API and efficiently download assets that aren't currently cached, saving your
user data.

## How Precaching Work

When a web app is loaded for the first time `workbox-precaching` will
look at all the assets, remove any duplicates and download those assets to
the precaching cache while also saving some information about the `revision`
of the asset in `indexedDB`.

> TODO Diagram of assets being de-duped, stored in cache and info stored
> in indexeddb

All of this happens in the service worker's [install event](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers#Install_and_activate_populating_your_cache).

When a user later re-visits your web app and you have a new service worker with
different precached assets, `workbox-precaching` will look at the new list
and determine which assets are new and which assets exist but need updating,
based on the revision information.

> TODO Diagram show cache entry being same, new and one revision be altered

In this service workers install event, these assets will updated in the cache
and their revision details will be updated or added to indexedDB.

> TODO Diagram show new state of the world.

This new service worker won't be used until it's `activate` event, but once
this event is triggered, `workbox-precaching` will check for any old cached
assets and remove them from the cache and indexedDB.

> TODO Diagram removing old assets

Precache will do this each time you update your service worker code to ensure
the user has the latest assets, while only updating what is really needed.

## How to Generate the Precache List

You can pass in either an Array of strings, or an Array of objects into the
precaching methods of `workbox-precaching` like so:

```javascript
precaching.precache([
  '/styles/example.ac29.css',
  {
    url: '/scripts/example.be93.js',
  },
  {
    url: '/index.html',
    revision: 'as46',
  }
]);
```

The first two values in the list `/styles/example.ac29.css` and
`{ url: '/scripts/example.be93.js' }` are special in that they have revisioning
information **in** the URL itself. This is best practice for the web and for
assets you are doing this with, you can precache either of these ways.

For assets where you don't have revisioning information in the URL, you **need**
to add a revision property. This allows `workbox-precaching` to know when the
file has changed.

Workbox comes with a tool to help with generating this list, see
[workbox-build]() and [workbox-cli](). But this list of assets can be
generated anyway you want and you can add to the precache list multiple
times if files comes from different tools.

```javascript
// Revisioned files added via a glob
precacing.precache([
  '/styles/example-1.abcd.css',
  '/styles/example-2.1234.css',
  '/scripts/example-1.abcd.js',
  '/scripts/example-2.1234.js',
]);

// Precache entries from somewhere else
precaching.preache([
  {
    url: '/index.html',
    revision: 'abcd',
  }, {
    url: '/about.html',
    revision: '1234',
  }
]);
```

## How to Change the Precache Name

> TODO How can developer change cacheName for a precache controller vs
> with default cache names.
