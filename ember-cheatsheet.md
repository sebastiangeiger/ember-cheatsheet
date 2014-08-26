# Ember Cheatsheet

## Templates / Handlebars
###The Application Template: (Think layout.html.erb)

    <script type="text/x-handlebars">
      <header>..</header>
      {{outlet}} <-- this displays the route specific template
      <footer>..</footer>
    </script>

Omitting `data-template-name` seems to mean that the template reacts to `App.ApplicationRoute`.

###A route specific name: (Think album/show.html.erb)

    <script type="text/x-handlebars" id="album"></script>

Seems to do the same as:

    <script type="text/x-handlebars" data-template-name="album"></script>


The `album` in `data-template-name="album"` matches up with `App.AlbumRoute`.

### Handlebar expressions

A route will return a model in the `model: function()..` part.
Use `{{name}}` to access `model.name`.

If your `model:` hook returns an array: `{{#each}} .. {{/each}}`.
Use `{{name}}` to access `model[i].name`.

If you need to access a dynamic property from inside a html tag, use `{{bindAttr src=imageUrl}}`.

If `model` is a single object and `model.songs` is an array, you can use `{{#each songs}} ... {{name}} ...{{/each}}` to access `model.songs[i].name`.
`{{#each song in songs}} ... {{song.name}} ... {{/each}}` does the same.
An added bonus is that you can still access `model.artist` by doing `{{artist}}` inside the each block.
