# .subscribe()

Subscribe the requesting client socket to changes/deletions of one or more database records.

```js
Something.subscribe(req, ids);
```


### Usage

|   | Argument   | Type         | Details |
|---|:-----------|:------------:|---------|
| 1 | `req`      | ((req))      | The incoming socket request (`req`) containing the socket to subscribe.
| 2 | `ids`      | ((array))    | An array of record ids (primary key values).


When a client socket is subscribed to a record, it is a member of its "record room" aka "instance room".  That means it will receive all messages about that record which are broadcasted by `.publishUpdate()`, `.publishDestroy()`, `.publishAdd()`, `.publishRemove()`, and `.message()`.


### Example

```javascript
  subscribeToLouies: function (req, res) {
    if (!req.isSocket) {
      return res.badRequest('Only a client socket can subscribe to Louies.  You, sir or madame, appear to be an HTTP request.');
    }

    // Let's say our client socket has a problem with people named "louie".

    // First we'll find all users named "louie" (or "louis" even-- we should be thorough)
    User.find({ or: [{name: 'louie'},{name: 'louis'}] }).exec(function(err, usersNamedLouie){
      if (err) {
        return res.serverError(err);
      }

      // Now we'll use the ids we found to subscribe our client socket to each of these records.
      User.subscribe(req, _.pluck(usersNamedLouie, 'id'));

      // Now any time a user named "louie" or "louis" is modified or destroyed, our client socket
      // will receive a notification (as long as it stays connected anyways).

      // All done!  We could send down some data, but instead we send an empty response.
      // (although we're ok telling this vengeful client socket when our users get
      //  destroyed, it seems ill-advised to send him our Louies' sensitive user data.
      //  We don't want to help this guy to hunt them down in real life.)
      return res.ok();
    });
  }
```




### Notes

> + Be sure and check `req.isSocket === true` before passing in `req` to refer to the requesting socket.  The provided `req` must be from a socket request, not just any old HTTP request.
> + `.subscribe()` will only work with requests made over a socket.io connection (e.g. using `io.socket.get()`), *not* over an http connection (e.g. using `jQuery.get()`).  See the [sails.io.js socket client documentation](http://sailsjs.org/documentation/reference/web-sockets/socket-client) for information on using client sockets to send WebSockets/Socket.io messages with Sails.
> + This function does _not actually talk to the database_!  In fact, none of the resourceful pubsub methods do.  Remember: these are just a simplified abstraction layer built on top of the lower-level `sails.sockets` methods, designed to make your app cleaner and easier to debug by using conventional names for events/rooms/namespaces etc.




<docmeta name="displayName" value=".subscribe()">
<docmeta name="pageType" value="method">

