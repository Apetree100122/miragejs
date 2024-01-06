# Mirage .
JS[![npm version](https://badge.fury.io/js/miragejs.svg)](https://badge.fury.io/js/miragejs)![example  workflow(https://github.com/miragejs
/miragejs/actions/workflows)([github/workflows/ci.yml/badge.svg)]
A client-side server to develop test and prototype your JavaScript app.
#Documentation
--
Visit [miragejs.com](https://miragejs.com) to read the docs--Introduction
Mirage is a JavaScript library that lets frontend developers mock out backend APIs.
Unlike other mocking libraries, Mirage makes it easy to recreate dynamic scenarios, the kind that are typically only possible when using a real production server.
Nearly all
JavaScript apps interact with an HTTP API. When you reach the point during development where you need to work with dynamic server data, you have a few options:  
Proxy to a local 
or hosted version of your actual backend. This can work if you already have an API, but you often don't. And even if you do, a lot of times you'll want to work with server state that's different from what's on 
the live API. Comment out your app's network requests and replace them with dummy data. This is the fastest option, but it forces you to deal with networking issues down the line, after you've already written a lot of application code.  Use a client-side interceptor to handle your app's network requests. Some HTTP clients come with mock adapters (e.g. axios-mock-adapter can be used to mock requests made with axios), and there are also standalone tools like Pretender you can use to intercept your app's network requests in the browser. This is the most flexible approach, but it requires you to start from scratch in each project, and leaves it up to you to enforce conventions across your apps.
Mirage was built to solve these problems. It's a fake server that runs in the client, it can be used in both development and testing, and it brings along enough conventions to get you up and running quickly.
How it works Mirage runs in the browser. It intercepts any XMLHttpRequest or fetch requests your JavaScript app makes and lets you mock the response. That means you can develop and test your app just as if it were talking to a real server. Let's say we were working on this React component:
````
// App.js
import React, { useState, useEffect } from
"react"
export function App() {
  let [users, setUsers] = useState([])
  useEffect(() => {
    fetch("/api/users")
      .then((response) => response.json())
      .then((json) => setUsers(json))  }, [])
  return ( <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}  </ul> )
}We use a simple Mirage route handler to handle the /api/users network request it makes, like this:// App.js
import React, { useState, useEffect } from "react"
import { createServer } from "miragejs"
createServer({  routes() { this.get("/api/users", () => [   { id: "1", name: "Luke" }, { id: "2", name: "Leia" }, { id: "3", name: "Anakin" }, ])
  },
})export function App() {
  let [users, setUsers] = useState([]) useEffect(() => { fetch("/api/users").then((response) => response.json()) .then((json) => setUsers(json)) }, [])return (  <ul> {users.map((user) => ( <li key={user.id}>{user.name}</li> ))}
    </ul>  )
}
````
Importantly, because Mirage mocks the HTTP boundary instead of the JavaScript code your app uses to make network requests, you never need to modify your UI code to account for whether your app is talking to Mirage or to your real production backend.
In addition to intercepting HTTP requests,Mirage provides a mock database and helper functions that make it easy to simulate dynamic backend services.
Mirage borrows concepts from typical server-side frameworks like
````
  routes to handle HTTP requests
    a database and models
 for storing data and defining relationships
    factories and fixtures
for stubbing data, and
    serializers for formatting
 HTTP responses to help you quickly configure your mock server.

To add Mirage to your project, run
# Using npm

`npm install -
-save-dev mirage.js`

# Using Yarn
`yarn add -
-dev miragejs`
