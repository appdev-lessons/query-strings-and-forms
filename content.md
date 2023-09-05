# Query strings and forms

We use forms to collect information from users. Only by submitting this information to our application, and processing it, does our application start to become interesting.

## A bare bones HTML form

```erb
<form>
</form>
```

### Required elements and attributes

A form should, at a minimum, have an `action` attribute and a descendant `<button>`.

The value of this `action` attribute is the URL that the user will be sent to when the button is clicked:

```html
<form action="https://en.wikipedia.org/wiki/Chicago">
	<button>Go to Wikipedia</button>
</form>
```
{: .repl #form_as_hyperlink title="form as hyperlink" points="1"}

At this point, the `<form>` is just a hyperlink, like an `<a>` tag.

The point of using `<form>` tags rather than `<a>` tags is that they can also capture user input using `<input>` elements. Each `<input>` must be a descendant element of the form.

```html
<form action="https://en.wikipedia.org/wiki/Chicago">
  <input type="text">

	<button>Go to Wikipedia</button>
</form>
```
{: .repl #first_input title="first input" points="1"}

<aside markdown="1">
`<input>` elements are one of the few HTML elements that don't require a closing tag.
</aside>

The `type` attribute determines what kind of field it is. `text` is the most common value for `type`  (and the default if you leave it out) . Try the values `radio`, `checkbox`, `password`, and `file` to get a sense of other options.

However, without any other attributes besides `type`, the `<input>` doesn't do anything.

### `<input>` attributes

At a minimum, each `<input>` needs a `name` attribute:

```html
<form action="https://en.wikipedia.org/wiki/Chicago">
  <input type="text" name="zebra">

	<button>Go to Wikipedia</button>
</form>
```

Try submitting this form and see what happens. Hint: pay particular attention to the URL you end up on.

The form now assembles a **query string** and tacks it on to the end of the URL in the `action` before placing the request.

Note: The value for the `name` attribute can be anything, but it should be unique within the form.

```html
<!-- GOOD: each input name is unique -->

<form action="/process_form">
  <input name="zebra" type="text">
  <input name="giraffe" type="password">
  <button>Submit</button>
</form>
```

```html
<!-- BAD: input names are not unique -->

<form action="/process_form">
  <input name="zebra" type="text">
  <input name="zebra" type="password">
  <button>Submit</button>
</form>
```

## Query strings

The query string is an optional part of a URL that begins with a `?`. Since it's optional, you can add a query string to the end of any URL:

<span style="font-size: 1.2rem;font-family: monospace;">http://www.example.com/<span style="color: red; text-decoration: underline;">?fruit=apple&color=green</span></span>

A query string functions similar to a Ruby Hash — it's a list of key-value pairs. In the example, `fruit` and `color` are keys, while `apple` and `green` are the values. 

Forms are useful because they _automatically_ create a query string when they are submitted. The keys in the resulting query string come from the `name` attribute of the `<input>` while the `value` comes from whatever the user typed into that field. The URL before query string comes from the `action` attribute of the `<form>`.

Try submitting the following form:

```html
<form action="https://en.wikipedia.org/wiki/Chicago">
  <input type="text" name="zebra">
  <input type="text" name="giraffe">

	<button>Go to Wikipedia</button>
</form>
```
  
So when a form is submitted, the user is sent to the URL that was specified in the `action` attribute of the form. The name-value pairs from each input field inside the `<form>` element are added as a query string onto the URL from the `action`. Try it out!

## Demo

<div class="iframe-container" style="overflow: hidden;padding-top: 56.25%;position: relative;"><iframe loading="lazy" style="border: 0;height: 100%;left: 0;position: absolute;top: 0;width: 100%;" src="https://jelani.dev/form-demo/"></iframe></div>

## params

In Sinatra and Rails apps, `params` is a special Hash that is defined every time someone places a request to a route. Sinatra and Rails will first check to see if a query string is present in the resource path, and if it finds one, it adds any name/value pairs from the query string into the `params` hash. Then, we can use the user inputs in our action.

Assuming our application has a route defined for `/add_fruit` and a user submitted a form that resulted in visit to this URL:

<span style="font-size: 1.2rem;font-family: monospace;">/add_fruit<span style="color: red;">?fruit=apple&color=green</span></span>

Sinatra and Rails read the query string and adds each name/value pair into the `params` Hash:

<span style="font-size: 1.2rem;font-family: monospace;">{<span style="color: red; ">&quot;fruit&quot; <span style="color:black;">=></span> &quot;apple&quot;<span style="color:black;">,</span> &quot;color&quot; <span style="color:black;">=></span> &quot;green&quot;</span>}</span>

Try it out!

### params demo

<div class="iframe-container" style="overflow: hidden;padding-top: 56.25%;position: relative;"><iframe loading="lazy" style="border: 0;height: 100%;left: 0;position: absolute;top: 0;width: 100%;" src="https://jelani.dev/form-demo/params"></iframe></div>

## Valid forms

While the previous forms were all _functional_ in that they created a query string with all of the information that a user filled out, they were technically invalid.

In order for a form to be considered valid by HTML standards, it must have a `<label>` element that is connected to each `<input>` element that the user interacts with.

First, add the `<label>` elements for each `<input>`:

```html
<form action="/process_url">
  <label>Fruit</label>
  <input name="fruit">
	
  <label>Color</label>
  <input name="color">
	
  <button>Submit</button>
</form>
```
{: .repl #labels_1 title="labels, 1" points="1"}

Then, in order to connect one `<label>` to one `<input>`, both the `<label>` and `<input>` need an attribute with the **same** unique value.

The `<label>` element needs a `for=""` attribute while the `<input>` element needs an `id=""` attribute.

The value of these attributes can be anything, but remember it must be unique.

A complete valid form looks like this:

```html
<form action="/process_url">
  <label for="zebra">Fruit</label>
  <input id="zenra" name="fruit">
	
  <label for="giraffe">Color</label>
  <input id="giraffe" name="color">
	
  <button>Submit</button>
</form>
```
{: .repl #labels_2 title="labels, 2" points="1"}

### Why use valid forms?

It seems like Chrome can deal with forms even if they are invalid — i.e., even if `<input>`s don't have `id`s and there aren't `<label>`s with corresponding `for` attributes. So why bother with them?

There are several reasons we need to write valid forms in our HTML:

- For users visiting our app from a phone, it's hard to tap on small fields like checkboxes. In valids forms, if you click on a label, it checks/unchecks the connected checkbox.
- Screen readers need the `for=""` and `id=""` attributes in order to read and fill out forms correctly.
- Search engines penalize you if the forms on your site are invalid.
- The tests for `rake grade` will fail, since they won't know how to fill out your forms.

### Pre-populating inputs

You can also set the initial content of a field using the `value` attribute:

```html
<form action="/process_form">
  <input name="email" value="alice@example.com">
  <button>Submit</button>
</form>
```
{: .repl #prepopulating_values title="Prepopulating values" points="1"}

This is especially useful for creating forms that helps users edit existing values.

### Placeholders

You can give the user some hints using the `placeholder` attribute:

```html
<form action="/process_form">
  <input name="email" palceholder="Enter your email...">
  <button>Submit</button>
</form>
```
{: .repl #placeholders title="placeholders" points="1"}

Unlike `value`, `placeholder` has no functional effect on the query string/params. It's just a hint for the user.

---

- Did you read this lesson without skimming?
- Yes
	- Great!
- No
	- Oh no! Query strings and forms are absolutely crucial concepts. Please re-read it when you have more time.
{: .choose_best #did_you_read title="Did you read it?" points="2" answer="1" }
