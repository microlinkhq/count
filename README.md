# count

![Last version](https://img.shields.io/github/tag/Kikobeats/count.svg?style=flat-square)
[![Build Status](https://img.shields.io/travis/com/Kikobeats/count/master.svg?style=flat-square)](https://travis-ci.com/Kikobeats/count)
[![Dependency status](https://img.shields.io/david/Kikobeats/count.svg?style=flat-square)](https://david-dm.org/Kikobeats/count)
[![Dev Dependencies Status](https://img.shields.io/david/dev/Kikobeats/count.svg?style=flat-square)](https://david-dm.org/Kikobeats/count#info=devDependencies)

A simple microservice for counting things.

![](demo.gif)

It's inspired by [antirez.com](http://antirez.com) and [rauchg.com](https://rauchg.com/).

[![Deploy with Vercel](https://zeit.co/button)](https://vercel.com/new/project?template=https://github.com/Kikobeats/count)

# Usage

You can count whatever you want.

Imagine you are visiting a blog post and you want to count views.

Just pass the blog post relative path for counting pageviews:

```
kikobeats.com/culture-shipping → count.now.sh/pageviews/culture-shipping?incr → HTTP 201 Created
```

The service will return you the current counter, and the first and last timestamp associated:

```json
{
  "updatedAt": 1564327678670,
  "createdAt": 1564327676653,
  "count": 25
}
```

For counting thing, you need to perform a `GET` to `count.now.sh/:collection/:id`, being supported the following query string:

- **increment**|**incr**: When it is present, it specifies how much increment the value.

It doesn't matter if your `id` contains final slash; it will be sanitized.

For printing the value in your website, a representative code for doing that could be:

```html
<script>
  fetch('https://count.now.sh' + window.location.pathname + '?increment&collection=pageviews)
    .then(res => res.json())
    .then(({ pageviews }) => {
      document.querySelector('.pageviews-container').classList.remove('display-none')
      document.querySelector('.pageviews-count').textContent = pageviews
    })
</script>
```

# Environment Variables

## Database

The service is backed at [Firebase](https://firebase.com/). You need to create an account (it's free!) and copy some relevant values you can find in your [Firebase accounts tab](https://console.firebase.google.com/project/_/settings/serviceaccounts/adminsdk).

### FIRESTORE_PROJECT_ID

Type: `string`

The project id to use.

### FIRESTORE_PRIVATE_KEY

Type: `string`

Your private key associated with your Firebase account.

It should be encoded in base64.

### FIRESTORE_CLIENT_EMAIL

Type: `string`

Your email associated with your Firebase account.

## Security

The following environment variables are optional and they are used protecting abusive use of the service from external traffic.

### API_KEY

Type: `string`

When you provide it, all request to need to be authenticated in order to increment counters.

The API key can be provided:

- as `x-api-key` request header.
- as `key` or `apiKey` as query parameter.

You can use [randomkeygen.com](https://randomkeygen.com) for generating an unique API key.

### DOMAINS

Type: `string`|`string[]`

The list of allowed domains that authorized to interact with the service.

## Rate Limit

The following variables are optional and providing them enable  a rate limit window associated per request IP address.

### RATE_LIMIT_WINDOW

Type: `number`

It defines a rate limit window, where the value specified is the number of seconds before `RATE_LIMIT_MAX` points are reset.

The value should be expressed in milliseconds.

### RATE_LIMIT_MAX

Type: `number`

It defines a rate limit window, where the value is the maximum number of points can be consumed over `RATE_LIMIT_WINDOW`.

## License

**count-microservice** © [Kiko Beats](https://kikobeats.com), released under the [MIT](https://github.com/Kikobeats/count/blob/master/LICENSE.md) License.<br>
Authored and maintained by Kiko Beats with help from [contributors](https://github.com/Kikobeats/count/contributors).

> [kikobeats.com](https://kikobeats.com) · GitHub [Kiko Beats](https://github.com/Kikobeats) · Twitter [@Kikobeats](https://twitter.com/Kikobeats)
