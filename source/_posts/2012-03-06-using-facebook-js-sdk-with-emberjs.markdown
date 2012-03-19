---
layout: post
title: "Using Facebook JS SDK with Ember.js"
date: 2012-03-06 10:25
comments: true
categories:
  - JavaScript
  - Ember.js
  - CoffeeScript
---

{% raw %}

This is my first blog post and it will be as simple as that.

### Using the ember-facebook library

I wrote a (_really_) simples addon for [Ember.js](http://www.emberjs.com) to automatically load the [facebook javascript sdk](https://developers.facebook.com/docs/reference/javascript/), it's called [ember-facebook](http://www.github.com/luan/ember-facebook) as you'd expect.

To use it you just have to download the compiled js file from the downloads page on the github repository. The library is written in [CoffeeScript](http://coffeescript.org) and I do [love CoffeeScript](http://arcturo.github.com/library/coffeescript/).

[Download ember-facebook.js](https://github.com/downloads/luan/ember-facebook/ember-facebook.js)

Once you done it include it right after the actual Ember.js file, and before your application one, do it as you are used to.

Right now you can use it like this, on your application creation include the Facebook Mixin:

```coffeescript Application Configuration
App = Em.Application.create(Em.Facebook)
App.set 'appId', 'YourAppId'
```

<!-- more -->

And it will graciously load the Facebook JS SDK for you, you don't have to do anything else. Now you are able to use the `FB` object to call Facebook's API, and we assigned a FBUser to your App to reffer to the logged in user.
If you want a login button you can do something like this:

```html Simple login/logout buttons
{{#if App.FBUser}}
  <div>
    <img {{bindAttr src="App.FBUser.picture" alt="App.FBUser.name"}} />
    {{App.FBUser.name}}
    <a href="#" {{action "logout" target="FB"}}>Logout</a>
  </div>
{{else}}
  {{#view Em.FacebookView type="login-button" data-size="xlarge"
                          data-scope="email, offline_access"}}
    Connect
  {{/view}}
{{/if}}
```

Note that we are checking for the existance of App.FBUser and then displaying the user names and picture together with a logout link if the user is present OR we show a custom view called FacebookView that basically creates a view of the Facebook SDK, you can check more on their documentation.

Basically we create a button with the xlarge size and ask for the scopes "email" and "offline_access" with the content "Connect".

You can customize it however. You could either create a link to that, just bindint the action "login" to the target "FB" (as we did with logout).

### Creating the ember-facebook library

The [sources](https://github.com/luan/ember-facebook/blob/master/src/ember-facebook.coffee) for the library are really really simple. I basically observes the "appId" attribute and when that changes it loads the Facebook JS SDK for that app.

Whenever the user changes (login, logout, app authorization, etc) the method `updateFBUser` is called, updating the App.FBUser object on your application. You can do whatever you want with this binding, observe it, put it in the DOM, whatever.

We also have a App.FBloading binding which is a boolean that basically tells that we are waiting a facebook response.

You might want to show something based on this:

```html Fancy Loading
{{#if App.FBloading}}
  <div class="my-fancy-loading">Waiting for Facebook</div>
{{/if}}
```

### That's all folks

I hope you like it, as it being my first post it might look retarded, but I hope it was helpful for someone.

See ya!

{% endraw %}
