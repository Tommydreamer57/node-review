
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
 - Event listener contains a method received from App through props
 - Event listener waits for the event to fire and then invokes the method received through props
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
 - When the matching endpoint is found, the function belonging to that endpoint is fired

### Endpoints

  method:   |  url:             |  body:                            |  response:
:-----------|:------------------|:----------------------------------|:----------------------
  GET       |  /api/words       |  null                             |  [ { value: 'word' } ]
  POST      |  /api/words       |  { words: [ { value: 'word' } ] } |  [ { value: 'word' } ]
  PUT       |  /api/words/:id   |  { value: 'word' }                |  [ { value: 'word' } ]
  DELETE    |  /api/words/:id   |  null                             |  [ { value: 'word' } ]

 - Endpoints are like event-listeners in our server, waiting for a matching request to come in
 - Like event-listeners, endpoints contain functions that will fire when a request is received


