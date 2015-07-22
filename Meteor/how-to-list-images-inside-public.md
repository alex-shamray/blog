# How to list images inside public/?

I want to list the available icons found in /public/icons/

It makes sense to use `Meteor.publish()` to return something static like a list of files in a folder. If I use `publish` I could easily use this data in several routes. With a method that lists the files in a folder, the client must call the server each time I want to show the list, which is not very efficient, and it would delay displaying the results. The list of files does not change every day. I would say that the application must feel faster if I provide the data as part of a route instead of calling a method each time.

So, it's possible to publish a list of files found in `/public`:

```javascript
Meteor.publish('icons', function() {
    var self = this;
    var fs = Npm.require("fs");
    var iconPath = process.env.PWD + '/public/icons/';
    var icons = fs.readdirSync(iconPath);
    _.each(icons, function(icon) {
        if(icon.indexOf('icon-') == 0) {
            self.added('icons', icon, { 'url': '/icons/' + icon });
        }
    });
    this.ready();
});
```

Then I include

```javascript
Meteor.subscribe('icons');
```

and

```javascript
Icons = new Mongo.Collection('icons');
```

which seems to act as a virtual collection, since it does not get saved to disk. But it works. In the client I can then query `Icons.find()`, which returns the list of icons found in `/public/icons`, bridging the filesystem to some kind of virtual collection I can query. I could even publish some metadata (size, date, type, ...) together with the url, and then query that metadata to get a subset of the icons.
