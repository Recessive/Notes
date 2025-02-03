---
tags:
  - webdev
---

Similar to [[REST]], but instead of accessing data by requesting from endpoints, you make a single request via a JSON containing keys of the data you want. This saves loading a bunch of URLs to get all the data you need.

Example request:

```
{
  user {
    name  
    height
  }
}
```
Which in rest would look something like:
```
GET /api/2.2/user/name HTTP/1.1
GET /api/2.2/user/height HTTP/1.1
```
Requiring 2 requests instead of 1. Lotta overhead