# GT Webdev 05 - NodeJS Tutorial

In this tutorial we'll learn about NodeJS, Express, EJS, and APIs

## 01: Initialize the project
Run the following in your command line and go through the onboarding:
```bash
npm init
```

## 02: Add a log to your server.js file and run it
```
console.log("Hello World!");
```
```bash
node server.js
```

## 03: Create a Script in your package.json file
```
"scripts": {
  "start": "babel-node server.js"
}
```
```bash
npm run start
```

## 04: Install Express
```bash
npm install express --save
```

## 05: Install the Babel-watch dev tool
```bash
npm i babel-watch --save-dev
```

## 06: Use this package.json and install all dependencies
```
{
  "name": "nodejs-tryit",
  "version": "1.0.0",
  "description": "Trying Node",
  "scripts": {
    "start": "babel-node server.js",
    "dev": "babel-watch server.js",
  },
  "author": "Jackson",
  "license": "ISC",
  "dependencies": {
    "babel-cli": "^6.26.0",
    "babel-core": "^6.26.0",
    "babel-preset-env": "^1.6.1",
    "body-parser": "^1.18.2",
    "ejs": "^2.5.7",
    "express": "^4.16.2"
  },
  "devDependencies": {
    "babel-watch": "^2.0.7"
  },
  "babel": {
    "presets": [
      "env"
    ]
  }
}
```
```bash
npm i
```

## 07: Import our dependencies in server.js
```
import express from 'express';
import path from 'path';
import bodyParser from 'body-parser';
```

## 08: Set up static serving
```
app.use(express.static(path.join(__dirname, 'public')));
```

## 09: Make the server listen
```
const PORT = 3000;
app.listen(PORT, () => {
  console.log('Listening on port ' + PORT);
});
```

## 10: EJS Route
```
let posts = [
  {
    "title": "Example Post",
    "body": "This is an example Body."
  }
];

app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

app.get('/', (req, res) => {
  res.render('index', { posts: posts });
});
```

## 11: Dynamic EJS Rendering
In the blank space of index.ejs
```
<% posts.forEach((post, index) => { %>
  <form method="POST" action="/">
    <input type="text" name="title" value="<%= post.title %>" />
    <textarea name="body"><%= post.body %></textarea>
    <input type="hidden" name="index" value="<%= index %>" />
    <div class="buttonRow">
      <input type="submit" value="Delete" name="delete" />
      <input type="submit" value="Update" name="update" />
    </div>
  </form>
<% }) %>
```

## 12: Handle Editing
```
app.use(bodyParser.urlencoded({ extended: true }))

...

app.post('/', (req, res) => {
  if (req.body.update) {
    posts[req.body.index] = {
      title: req.body.title,
      body: req.body.body
    }
  } else if (req.body.delete) {
    posts.splice(req.body.index, 1);
  } else {
    posts.push({
      title: req.body.title,
      body: req.body.body
    })
  }
  res.render('index', { posts: posts });
});
```

## 13: Read API
```
app.use(bodyParser.json())

...

app.get('/api/posts', (req, res) => {
  res.send(posts);
});
```

## 14: Read by id API
```
app.get('/api/posts/:id', (req, res) => {
  if (posts[req.params.id]) {
    res.send(posts[req.params.id])
  } else {
    res.status(404).send("Post Not Found.");
  }
});
```

## 15: Create API
```
app.post('/api/posts', (req, res) => {
  posts.push({
    title: req.body.title,
    body: req.body.body
  })
  res.status(201).send("Post Created")
});
```

## 16: Update API
```
app.put('/api/posts/:id', (req, res) => {
  if (posts[req.params.id]) {
    posts[req.params.id] = {
      title: req.body.title,
      body: req.body.body
    }
    res.send("Post Updated")
  } else {
    res.status(404).send("Post Not Found.");
  }
});
```

## 17: Delete API
```
app.delete('/api/posts/:id', (req, res) => {
  if (posts[req.params.id]) {
    posts.splice(req.body.index, 1);
    res.send("Post Deleted")
  } else {
    res.status(404).send("Post Not Found.");
  }
});
```