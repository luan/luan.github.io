---
layout: post
title: "Introducing Ember FormBuilder"
date: 2012-03-19 14:36
comments: true
categories:
  - JavaScript
  - Ember.js
  - CoffeeScript
  - Forms
---

{% raw %}

I was hacking around with Ember.js and had to do a kind of complex form.
All fields were bound to one object and I had a hard time styling and binding everything.
While doing this I got myself thinking of [simple_form](https://github.com/plataformatec/simple_form)
and how it would be helping me if I could use it.

And why not create a form builder for Ember? Yeah why not, I decided to try. I can't say I failed
miserably because the idea is cool, I like what I wrote in the [README](https://github.com/luan/ember-formbuilder/blob/master/README.md)
and how things are defined to work. But no doubt the code ended up a pile of crap,
complicated, hard to understand and hard to evolve from.

<!-- more -->

This post is about the idea. And to tell you guys that I will now rewrite it (yeah I know,
I'm rewriting a project that has what, 2 days life? But one should fail ASAP).

Any help is appreciated, I'm really new to testing javascript (and to testing at all)
but I'm finding my way in that. Now let's talk about the form builder.

# Ember FormBuilder

Based on simple_form primarily Ember FormBuilder aims to be the toolkit to build from basic to
complex forms that binds automatically to your ember objects. It's really customizable by just
defining a mixin with your markup definitions, just like this:

```coffeescript
Bootstrap = Ember.Mixin.create
  wrapperTag: 'div'
  wrapperClass: 'control-group'
  inputWrapperTag: 'div'
  inputWrapperClass: 'controls'
  labelClass: 'control-label'
  helpTag: 'p'
  helpClass: 'help-block'
  errorTag: 'span'
  errorClass: 'help-inline'
  formClass: 'form-vertical'
  submitClass: 'btn btn-success'
  cancelClass: 'btn btn-danger'
Ember.FormBuilder.pushMixin(Bootstrap, "bootstrap")
```

If you are familiar to simple_form syntax the handlebars helpers we define will be just the
thing you were looking for. You can define a form with `{{#formFor "object"}}` and define
imputs with `{{input "name"}}`. As simple as that. You can find out more about it by reading
the [README](https://github.com/luan/ember-formbuilder/blob/master/README.md) on github.

There is also support for some kind of internationalization, I say somekind because you
set an object `Ember.FormBuilder.STRINGS` with some string values (again refer to README
here) and you have to work your way to make it internationalized in there, it's something
to be thought further later, but that's another subject for another time.

# Ok

Yeah I mean, that's the whole thing. I just wanted to show that. I will rewrite it now
using decent patterns and making things more reusable and easier to understand. Will
write decent unit tests and integration tests too, currently there is just a bunch of
weird tests that I don't belive that covers much of it.

The code is in github [here](https://github.com/luan/ember-formbuilder) and if you
have time and want to help I would be delighted. Of course, read the README and
point out flaws if you see some.

Thanks for reading, good bye.

{% endraw %}
