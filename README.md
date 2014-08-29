# Ember Cheatsheet

## 1. Templates / Handlebars
### 1.1 The Application Template: (Think layout.html.erb)

    <script type="text/x-handlebars">
      <header>..</header>
      {{outlet}} <-- this displays the route specific template (Rails: yield)
      {{render "someTemplate"}} <-- this renders "someTemplate" (Rails: partials)
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
Use `{{name}}` inside the each block to access `model[i].name`.

If you need to access a dynamic property from inside a html tag, use `{{bindAttr src=imageUrl}}`.

If `model` is a single object and `model.songs` is an array, you can use `{{#each songs}} ... {{name}} ...{{/each}}` to access `model.songs[i].name`.

    {{#each songs}} ... {{name}} ...{{/each}}

does the same as:

    {{#each song in songs}} ... {{song.name}} ... {{/each}}

An added bonus is that you can still access `model.artist` by doing `{{artist}}` inside the each block.

### 1.4 Handlebar helpers
Define a helper:

	Ember.Handlebars.helper('format-something', function(value){
		return value.toLowerCase();
	})

Use it:

	{{format-something variable}}


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

### 2.1 Routes vs Resources
If you define `resource('album',...)` you will need to create an `App.AlbumRoute`.

If you nest resources the name of the route does not change.

    App.Router.map(function() {
      this.resource('artist', function(){
        this.resource('album'); // Still App.AlbumRoute
      });
    });

Use `route` to refine a resource, e.g.: 
  
    this.resource('album', function(){
      this.route('edit'); // -> App.AlbumEditRoute
     })
    
Using `this.resource('edit')` would be bad because the you have a global `App.EditRoute`.

TODO: Don't know how to do index and show routes (similar to rails)


## 3. Models
### 3.1 Models
Define a class:

	App.Album = Ember.Object.extend();
	
Create an instance of that class:

	var myAlbum = App.Album.create({
		title: "(What's the Story) Morning Glory?"
	})
Get a property:
	
	myAlbum.get('title')
	
Set a property (use this over `myalbum.title = 'Something'`, Ember will not pick up the changes)
	
	myAlbum.set('title', 'Definitely Maybe')

Define a computed property:

	App.Album = Ember.Object.extend({
		someProperty: function(){
			return this.get('p1') + this.get('p2'); //someProperty is dependent on p1 and p2
		}.property('p1','p2'); //Help ember by annotating the dependencies
	});
	
Advanced computed properties, if your computed property depends on an array:

	App.Album = Ember.Object.extend({
		someProperty: function(){
			return this.get('p1').reduce(function(memory,item){]
				return memory + item.get('value'); //someProperty is dependent on value of all p1s
				})
		}.property('p1.@each.value'); //Help ember by annotating the dependencies
	});





### 3.2 Collections

  * Search in collections using `collection.findProperty('id', someId)` with `collection = {[id: 1, name: "Alfred"], [id: 2, name: "Berta"]}`.

## 4. Tools

  * [JSBin for Ember](http://emberjs.jsbin.com/)
  * [Ember Inspector for Chrome](https://chrome.google.com/webstore/detail/ember-inspector/bmdblncegkenkacieihfhpjfppoconhi?hl=en)
