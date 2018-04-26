
# Data Flow with React and Node

## Font end

### Component tree

 - App
     - Header
     - Content
         - Display
         - Input
     - Footer


 - App has two children - Header and Content
 - Content has two children - Display and Input

### State

 - App keeps track of state (data) - `{ key: 'value' }`
    and contains methods (functions) - `methodName() {}`
    to interact with state - `this.setState({})`

### Props

 - App passes data and methods down to its children on props - `<Child props={props} />`
     - App passes data on props to children that display that data (Header, Content)
     - App also passes methods on props to children that need to interact with that data (Content)
 - Header receives data to display
 - Content receives data and methods to pass down to Display and Input
 - Display receives data to display
 - Input receives data and methods to both display data and allow user to interact with data

### Event Listeners

 - Input contains an event listener - `onChange` / `onClick` / `onMouseOver` / etc...
 - Event listeners contain a method received from App through props
 - Event listeners wait for the event to fire and then invoke the method received through props
 - The method received through props updates the state of the app
 - Updating the state of the app updates all components that access that state

### Http Requests

 - Methods can send http requests to interact with the server
 - Requests can be sent at certain lifecycles in React - `componentDidMount` / `componentWillUnmount` / etc...
 - Or they can be sent at certain events in the Input - `onChange` / `onClick` / `onMouseOver` / etc...

### Promises

 - Http requests return promises
 - Promises can have one of three statuses - pending, fulfilled, or rejected
 - Promises have methods that can be used to fire certain functions when the status of the request changes
 - Promise.then() takes a callback function that will fire when the request is fulfilled
     - The callback of the `.then()` method has access to the response object through its first parameter
 - Promise.catch() takes a callback function that will fire when the request is rejected
     - The callback of the `.catch()` method has access to the error through its first parameter

## Back End

### Server

 - The server is like a massive event-listener waiting for requests
 - Until a request is received, the server just sits and waits
 - When a request is received, the server runs through it's endpoints to find a matching method and url
 - When the matching endpoint is found, the callback function belonging to that endpoint is fired

### Middleware

 - Top-level middlewares are functions that the app will invoke on each request before sending the request to its matching endpoint
 - BodyParser.json() can be used as a middleware to give our endpoints easy access to the body of any request

### Endpoints

  method:   |  url:             |  body:                            |  response:
:-----------|:------------------|:----------------------------------|:----------------------
  GET       |  /api/words       |  null                             |  [ { value: 'word' } ]
  POST      |  /api/words       |  { words: [ { value: 'word' } ] } |  [ { value: 'word' } ]
  PUT       |  /api/words/:id   |  { value: 'word' }                |  [ { value: 'word' } ]
  DELETE    |  /api/words/:id   |  null                             |  [ { value: 'word' } ]

 - Endpoints are like event-listeners in our server, waiting for a matching request to come in
 - Like event-listeners, endpoints contain callback functions that will fire when a request is received

### Controllers

 - A controller is an object that contains the callbacks for each endpoint
 - This pattern keeps code clean and separates concerns
 - Multiple controllers can be used to further separate concerns between groups of endpoins

### Responses

 - Each callback has access to both the request and the response objects through its first and second parameters, respectively
 - Parameters from the url can be accessed through `request.params[parameterName]`
 - With body-parser, the body of the request is accessable through `request.body`
 - After any logic has been performed, the status can be set trough `response.status(statusCode)`
 - Data can be sent with the response through chaining onto the status - `response.status(statusCode).send(data)

### Promises

 - Once the response is sent, the promise on the front end will be either fulfilled or rejected depending on the status code
 - If a good status code (200, 202, etc...) is sent, the promise will be fulfilled and the `.then()` callbacks will be fired
 - If a bad status code (400, 500, etc...) is sent, the promise will be rejected and the `.catch()` callbacks will be fired

## More on Promises...

 - Promise.then() and Promise.catch() can be chained any number of times in order to invoke multiple functions when a promise is fulfilled
 - Each promise keeps track of all `.then()` and `.catch()` callbacks in order
     - The first `.then()` callback will be invoked with the response received from the request
     - Each following `.then()` callback will be invoked with the `return` of the previous callback
 - If an error is thrown inside any `.then()` callbacks, the `.then()` chain will be ended and the `.catch()` callbacks will then be fired
