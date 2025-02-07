---
tags:
  - concept
  - webdev/html
aliases:
  - Cascading Style Sheets
---
Defines styles for a HTML page.

To utilize [[CSS]], the HTML will contain a `<link>` [[Tag]] pointing to a [[CSS]] file. This will cause your browser to send a [[HTTP GET|GET]] request for the specified resource:

```html
<link rel="stylesheet" type="text/css" href="/exampleapp/main.css" />
```

The `rel` [[Attribute]] tells the browser the kind of relationship this resource has to the document. In this case, it is a stylesheet.