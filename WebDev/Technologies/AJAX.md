---
tags:
  - webdev/technology
  - js
aliases:
  - Asynchronous JavaScript and XML
---
Revolutionized webdev by allowing dynamic webpages that didn't require refreshing to update. Before this technology, most pages functioned as a [[Traditional Web App]] where all HTML on a page was server-generated.


## JavaScript

The server sends JavaScript in a `<script>` tag:
```html
<script type="text/javascript" src="/exampleapp/main.js"></script>
```
This triggers your browser to send a [[HTTP GET|GET]] request to the server to fetch the JavaScript at the location in the `src` [[Attribute]]. 
The moment it is loaded it is run.

Some traditional JavaScript could look like the following:
```js
var xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    const data = JSON.parse(this.responseText)
    console.log(data)

    var ul = document.createElement('ul')
    ul.setAttribute('class', 'notes')

    data.forEach(function(note) {
      var li = document.createElement('li')

      ul.appendChild(li)
      li.appendChild(document.createTextNode(note.content))
    })

    document.getElementById('notes').appendChild(ul)
  }
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```

This code creates an event handler and waits on a fetch to `/data.json`, and once completed runs the code bound to `onreadystatechange`. This code creates an **Unordered List** `<ul>` [[WebDev/Concepts/Element]], and iterates over the data to create a **List Element** `<li>` [[WebDev/Concepts/Element]] for each item. Wherever the parent `<script>` [[Tag]] is that contains this code (specified by the `src` [[Attribute]]) will be where this list is placed in the [[WebDev/Concepts/Document Object Model|DOM]].

The JavaScript here is directly modifying the [[WebDev/Concepts/Document Object Model|DOM]] by using `document`. The top node of the [[WebDev/Concepts/Document Object Model|DOM]] is called *document*.

