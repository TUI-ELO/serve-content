# Basic routing

_Routing_ refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on).

## Dit stuk is nieuw!

*   Eerste nieuwe list item
*   Tweede nieuwe list item
*   Derde nieuwe list item

Each route can have one or more handler functions, which are executed when the route is matched.

Route definition takes the following structure:

```
app.METHOD(PATH, HANDLER)
```

Where:

*   `app` is an instance of `express`.
*   `METHOD` is an [HTTP request method](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods), in lowercase.
*   `PATH` is a path on the server.
*   `HANDLER` is the function executed when the route is matched.

This tutorial assumes that an instance of `express` named `app` is created and the server is running. If you are not familiar with creating an app and starting it, see the [Hello world example](https://expressjs.com/en/starter/hello-world.html).

The following examples illustrate defining simple routes.

Respond with `Hello World!` on the homepage:

```
app.get('/', function (req, res) {
  res.send('Hello World!')
})
```

Respond to POST request on the root route (`/`), the applicationâ€™s home page:

```
app.post('/', function (req, res) {
  res.send('Got a POST request')
})
```

Respond to a PUT request to the `/user` route:

```
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})
```

<CodeExercise id="wrb9sGzjLz3aaaslyAgg" title="Express.js vraag" language="javascript" slider>
  
  De volgende vraag is om in te vullen.
  
  Laat je mooiste stukje code zien.
  
</CodeExercise>

Respond to a DELETE request to the `/user` route:

```
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user')
})
```

<Note>

De volgende video is verplicht om bekeken te hebben voor de volgende les.
  
<Video url="https://www.youtube.com/watch?v=L72fhGm1tfE" />
</Note>

For more details about routing, see the [routing 
<<<<<<<<<
 guide](https://expressjs.com/en/guide/routing.html).
 
=========
 guide](https://expressjs.com/en/guide/routing.html).

Tot slot heb ik hier nog wat nieuws toegevoegd. 
>>>>>>>>>
