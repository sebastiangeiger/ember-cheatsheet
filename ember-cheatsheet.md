# Ember Cheatsheet

## 1. Templates / Handlebars
### 1.1 The Application Template: (Think layout.html.erb)

    <script type="text/x-handlebars">
      <header>..</header>
      {{outlet}} <-- this displays the route specific template
      <footer>..</footer>
    </script>

Omitting `data-template-name` seems to mean that the template reacts to `App.ApplicationRoute`.

### 1.2 Route specific Template: (Think album/show.html.erb)

    <script type="text/x-handlebars" id="album"></script>

Seems to do the same as:

    <script type="text/x-handlebars" data-template-name="album"></script>


The `album` in `data-template-name="album"` matches up with `App.AlbumRoute`.

### 1.3 Handlebar expressions

A route will return a model in the `model: function()..` part.
Use `{{name}}` to access `model.name`.

If your `model:` hook returns an array: `{{#each}} .. {{/each}}`.
Use `{{name}}` to access `model[i].name`.

If you need to access a dynamic property from inside a html tag, use `{{bindAttr src=imageUrl}}`.

If `model` is a single object and `model.songs` is an array, you can use `{{#each songs}} ... {{name}} ...{{/each}}` to access `model.songs[i].name`.

    {{#each songs}} ... {{name}} ...{{/each}}

does the same as:

    {{#each song in songs}} ... {{song.name}} ... {{/each}}

An added bonus is that you can still access `model.artist` by doing `{{artist}}` inside the each block.


## 2. Routes

`ApplicationRoute` responds to `/` if you haven't setup anything.

    App.ApplicationRoute = Ember.Route.extend({
      model: function() {
        return ...
      }
    });

You can map urls to routes by doing this:

    App.Router.map(function() {
      this.resource('album', { path: '/album/:bananas_id' }); //bananas for added clarity
    });

`album` here is the name of the resource.
That slug (here `:bananas_id`) is then put into a params object `params.bananas_id`.

You need to define a route for the `album` resource:

    App.AlbumRoute = Ember.Route.extend({
      model: function(params) {
        return ... //access params.bananas_id here
      }
    });

The slug needs to end with `_id`. (`album_id` is probably the more helpful naming, I used `:bananas_id` for better illustration).


## 3. Collections

  * Search in collections using `collection.findProperty('id', someId)` with `collection = {[id: 1, name: "Alfred"], [id: 2, name: "Berta"]}`.
