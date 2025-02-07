---
tags:
  - webdev/html
---
A form element is denoted by the `<form>` [[Tag]]. A form element might look like:
```html
<form action='/exampleapp/new_note' method='POST'>
	<input type="text" name="note">
	<br>
	<input type="submit" value="Save">
</form>
```

The `action` [[Attribute]] specifies the URL that should be accessed when the form is submitted, which occurs when the `input` [[WebDev/Concepts/Element]] with the `type` [[Attribute]] set to `"submit"` is activated.

The `name="note"` [[Attribute]] definition indicates when the form is submitted, the value in this `input` will have the name `note`. The `value="Save"` [[Attribute]] definition specifies that the text on the button will be "Save".

A form is constituted of a series `input` [[WebDev/Concepts/Element|Elements]] that either submit the form, or contribute to the data the form sends.