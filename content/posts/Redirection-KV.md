---
title: "Redirection avec KV"
date: 2020-05-28T10:17:49+01:00
draft: false
---

Voici un exemple de Workers utilisant REDIRECT qui est un KV Namespace contenant l'ensemble des chemins de redirection:


```
addEventListener('fetch', async event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  let requestURL = new URL(request.url)
  let path = requestURL.pathname
  let location = await REDIRECT.get(path)
  if (location) {
    return Response.redirect(location, 301)
  }
  // If not in KV, return the original request
  return fetch(request)
}
```

`REDIRECT` doit être attaché au Workers dans la partie Settings > KV Namespace Bindings

Le nom de la variable `REDIRECT` doit ensuite pointer vers le nom de la KV contenant les redirections, par example:


Key                   | Value
----------------------|--------------------------
`/redirect-workers/1` | `https://www.example.com`